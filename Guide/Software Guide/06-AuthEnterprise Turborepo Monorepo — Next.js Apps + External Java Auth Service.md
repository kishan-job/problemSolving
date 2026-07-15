# Enterprise Turborepo Monorepo — Next.js Apps + External Java Auth Service

A realistic example of a Turborepo monorepo with multiple Next.js apps, delegating all authentication to a separate Java backend service.

---

## 1. Overall Architecture

```
User Browser
    ↓
Turborepo Monorepo (Next.js apps)
├── apps/web        (main site — example.com)
├── apps/checkout   (checkout — example.com/checkout via Multi-Zone)
└── apps/admin      (admin panel — admin.example.com)
    ↓ (all apps call internal auth service)
Java Auth Service (SEPARATE repo, separate deploy — owned by backend team)
    ↓
User Database (Postgres/Oracle/etc.)
```

**Key points:**
- Java auth service lives in its OWN Git repo (not in the Turborepo)
- Turborepo contains only Next.js apps + shared frontend packages
- Java service owns password validation, JWT signing, session management
- Next.js apps are thin clients that call the Java service

---

## 2. Turborepo Structure

```
company-frontend-monorepo/          ← Turborepo (frontend only)
├── apps/
│   ├── web/                        ← Main Next.js app
│   ├── checkout/                   ← Checkout Next.js app
│   └── admin/                      ← Admin Next.js app
├── packages/
│   ├── auth-client/                ← Shared auth client (calls Java service)
│   ├── ui/                         ← Shared React components
│   ├── api-client/                 ← Shared API clients for other backend services
│   └── config/                     ← Shared configs (ESLint, TS, Tailwind)
├── turbo.json
├── package.json
└── pnpm-workspace.yaml

────────────────────────────────────

company-auth-service/                ← SEPARATE Git repo (Java backend)
├── src/main/java/com/company/auth/
│   ├── AuthController.java
│   ├── JwtService.java
│   ├── UserRepository.java
│   └── ...
├── pom.xml
└── Dockerfile
```

---

## 3. Turborepo Root Configuration

### 3.1 `package.json` (root)

```json
{
  "name": "company-frontend-monorepo",
  "private": true,
  "workspaces": ["apps/*", "packages/*"],
  "scripts": {
    "dev": "turbo run dev",
    "build": "turbo run build",
    "lint": "turbo run lint",
    "type-check": "turbo run type-check"
  },
  "devDependencies": {
    "turbo": "^2.0.0",
    "typescript": "^5.3.0"
  }
}
```

### 3.2 `pnpm-workspace.yaml`

```yaml
packages:
  - "apps/*"
  - "packages/*"
```

### 3.3 `turbo.json`

```json
{
  "$schema": "https://turbo.build/schema.json",
  "tasks": {
    "build": {
      "dependsOn": ["^build"],
      "outputs": [".next/**", "!.next/cache/**"]
    },
    "dev": {
      "cache": false,
      "persistent": true
    },
    "lint": {},
    "type-check": {
      "dependsOn": ["^build"]
    }
  }
}
```

---

## 4. Shared Auth Client Package

### 4.1 `packages/auth-client/package.json`

```json
{
  "name": "@company/auth-client",
  "version": "1.0.0",
  "main": "./src/index.ts",
  "types": "./src/index.ts",
  "dependencies": {
    "jose": "^5.0.0",
    "next": "^15.0.0"
  }
}
```

### 4.2 `packages/auth-client/src/index.ts`

```typescript
import { cookies } from 'next/headers';
import { jwtVerify, createRemoteJWKSet } from 'jose';

const AUTH_SERVICE_URL = process.env.AUTH_SERVICE_URL!;
// e.g., 'http://auth-service.internal:8080' (only reachable from Next.js server)

// Public keys from Java service for local JWT verification (fast)
const JWKS = createRemoteJWKSet(
  new URL(`${AUTH_SERVICE_URL}/auth/.well-known/jwks.json`)
);

// ─────────────────────────────────────────────────────
// LOGIN — called from Server Action in main web app
// ─────────────────────────────────────────────────────
export async function loginWithBackend(email: string, password: string) {
  const response = await fetch(`${AUTH_SERVICE_URL}/auth/login`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ email, password }),
  });
  
  if (!response.ok) {
    throw new Error('Invalid credentials');
  }
  
  const { accessToken, refreshToken, expiresIn } = await response.json();
  
  // Cookies scoped to ROOT domain so all Next.js apps can read
  cookies().set('access_token', accessToken, {
    domain: '.example.com',
    path: '/',
    httpOnly: true,
    secure: true,
    sameSite: 'lax',
    maxAge: expiresIn,
  });
  
  cookies().set('refresh_token', refreshToken, {
    domain: '.example.com',
    path: '/',
    httpOnly: true,
    secure: true,
    sameSite: 'lax',
    maxAge: 60 * 60 * 24 * 30,
  });
}

// ─────────────────────────────────────────────────────
// GET SESSION — used by ALL Next.js apps
// Uses local JWT verification with public key (fast)
// Falls back to refresh flow if token expired
// ─────────────────────────────────────────────────────
export async function getSession() {
  const accessToken = cookies().get('access_token')?.value;
  if (!accessToken) return null;
  
  try {
    const { payload } = await jwtVerify(accessToken, JWKS, {
      issuer: 'company-auth-service',
      audience: 'company-frontend',
    });
    
    return {
      userId: payload.userId as string,
      email: payload.email as string,
      roles: payload.roles as string[],
    };
  } catch (error: any) {
    if (error.code === 'ERR_JWT_EXPIRED') {
      // Try to refresh
      return await refreshSession();
    }
    return null;
  }
}

// ─────────────────────────────────────────────────────
// REFRESH — silent token refresh
// ─────────────────────────────────────────────────────
async function refreshSession() {
  const refreshToken = cookies().get('refresh_token')?.value;
  if (!refreshToken) return null;
  
  const response = await fetch(`${AUTH_SERVICE_URL}/auth/refresh`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ refreshToken }),
  });
  
  if (!response.ok) return null;
  
  const { accessToken, expiresIn } = await response.json();
  
  cookies().set('access_token', accessToken, {
    domain: '.example.com',
    path: '/',
    httpOnly: true,
    secure: true,
    sameSite: 'lax',
    maxAge: expiresIn,
  });
  
  const { payload } = await jwtVerify(accessToken, JWKS);
  return {
    userId: payload.userId as string,
    email: payload.email as string,
    roles: payload.roles as string[],
  };
}

// ─────────────────────────────────────────────────────
// LOGOUT
// ─────────────────────────────────────────────────────
export async function logout() {
  const accessToken = cookies().get('access_token')?.value;
  
  if (accessToken) {
    await fetch(`${AUTH_SERVICE_URL}/auth/logout`, {
      method: 'POST',
      headers: { 'Authorization': `Bearer ${accessToken}` },
    });
  }
  
  cookies().delete({ name: 'access_token', domain: '.example.com', path: '/' });
  cookies().delete({ name: 'refresh_token', domain: '.example.com', path: '/' });
}

// ─────────────────────────────────────────────────────
// GET AUTH TOKEN — for calling other backend services
// ─────────────────────────────────────────────────────
export function getAuthToken() {
  return cookies().get('access_token')?.value;
}
```

---

## 5. Main Web App (`apps/web/`)

### 5.1 `apps/web/package.json`

```json
{
  "name": "@company/web",
  "version": "1.0.0",
  "scripts": {
    "dev": "next dev -p 3000",
    "build": "next build",
    "start": "next start"
  },
  "dependencies": {
    "@company/auth-client": "workspace:*",
    "@company/ui": "workspace:*",
    "next": "^15.0.0",
    "react": "^19.0.0",
    "react-dom": "^19.0.0"
  }
}
```

### 5.2 `apps/web/next.config.js` — Multi-Zone Rewrites

```javascript
module.exports = {
  async rewrites() {
    return [
      {
        source: '/checkout/:path*',
        destination: 'https://checkout.example.com/checkout/:path*',
      },
    ];
  },
};
```

### 5.3 `apps/web/app/login/page.tsx`

```jsx
import { loginWithBackend } from '@company/auth-client';
import { redirect } from 'next/navigation';

export default function LoginPage() {
  async function login(formData: FormData) {
    'use server';
    
    try {
      await loginWithBackend(
        formData.get('email') as string,
        formData.get('password') as string
      );
      redirect('/dashboard');
    } catch {
      return { error: 'Invalid email or password' };
    }
  }
  
  return (
    <form action={login}>
      <input name="email" type="email" required />
      <input name="password" type="password" required />
      <button type="submit">Log in</button>
    </form>
  );
}
```

### 5.4 `apps/web/app/dashboard/page.tsx`

```jsx
import { getSession, getAuthToken } from '@company/auth-client';
import { redirect } from 'next/navigation';

export default async function DashboardPage() {
  const user = await getSession();
  if (!user) redirect('/login');
  
  // Fetch dashboard data from another backend service
  const dashboardData = await fetch('http://dashboard-service.internal/data', {
    headers: { Authorization: `Bearer ${getAuthToken()}` },
    cache: 'no-store',
  }).then(r => r.json());
  
  return (
    <div>
      <h1>Welcome, {user.email}</h1>
      <p>Role: {user.roles.join(', ')}</p>
      <a href="/checkout/cart">Go to checkout</a>
    </div>
  );
}
```

---

## 6. Checkout App (`apps/checkout/`)

### 6.1 `apps/checkout/package.json`

```json
{
  "name": "@company/checkout",
  "version": "1.0.0",
  "scripts": {
    "dev": "next dev -p 3001",
    "build": "next build",
    "start": "next start"
  },
  "dependencies": {
    "@company/auth-client": "workspace:*",
    "@company/ui": "workspace:*",
    "next": "^15.0.0",
    "react": "^19.0.0",
    "react-dom": "^19.0.0"
  }
}
```

### 6.2 `apps/checkout/next.config.js`

```javascript
module.exports = {
  basePath: '/checkout',
  assetPrefix: 'https://checkout.example.com',
};
```

### 6.3 `apps/checkout/middleware.ts`

```typescript
import { NextResponse } from 'next/server';
import type { NextRequest } from 'next/server';
import { jwtVerify, createRemoteJWKSet } from 'jose';

const AUTH_SERVICE_URL = process.env.AUTH_SERVICE_URL!;
const JWKS = createRemoteJWKSet(
  new URL(`${AUTH_SERVICE_URL}/auth/.well-known/jwks.json`)
);

export async function middleware(request: NextRequest) {
  const token = request.cookies.get('access_token')?.value;
  
  if (!token) {
    return NextResponse.redirect(new URL('https://example.com/login'));
  }
  
  try {
    await jwtVerify(token, JWKS);
    return NextResponse.next();
  } catch {
    return NextResponse.redirect(new URL('https://example.com/login'));
  }
}

export const config = {
  matcher: ['/checkout/:path*'],
};
```

### 6.4 `apps/checkout/app/checkout/cart/page.tsx`

```jsx
import { getSession, getAuthToken } from '@company/auth-client';
import { redirect } from 'next/navigation';

export default async function CartPage() {
  const user = await getSession();
  // Same function from shared package — reads same cookie set by web app
  
  if (!user) redirect('https://example.com/login');
  
  // Call cart microservice with user's JWT
  const cart = await fetch(`http://cart-service.internal/cart/${user.userId}`, {
    headers: { Authorization: `Bearer ${getAuthToken()}` },
    cache: 'no-store',
  }).then(r => r.json());
  
  return (
    <div>
      <h1>{user.email}'s Cart</h1>
      {cart.items.map(item => (
        <div key={item.id}>{item.name} — ${item.price}</div>
      ))}
    </div>
  );
}
```

---

## 7. Admin App (`apps/admin/`)

### 7.1 `apps/admin/app/users/page.tsx`

```jsx
import { getSession, getAuthToken } from '@company/auth-client';
import { redirect } from 'next/navigation';

export default async function UsersPage() {
  const user = await getSession();
  
  // Same session check, plus role-based access
  if (!user) redirect('https://example.com/login');
  if (!user.roles.includes('admin')) {
    return <div>Access denied</div>;
  }
  
  const users = await fetch('http://user-service.internal/users', {
    headers: { Authorization: `Bearer ${getAuthToken()}` },
  }).then(r => r.json());
  
  return (
    <div>
      <h1>User Management</h1>
      {users.map(u => <div key={u.id}>{u.email}</div>)}
    </div>
  );
}
```

---

## 8. The Java Auth Service (Separate Repo)

Included so you see how tokens are actually issued. This code lives in a **completely separate Git repo**, maintained by the backend team.

### 8.1 `AuthController.java`

```java
package com.company.auth.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/auth")
public class AuthController {
    
    @Autowired private UserRepository userRepo;
    @Autowired private JwtService jwtService;
    @Autowired private PasswordEncoder passwordEncoder;
    @Autowired private TokenBlocklistService blocklist;
    
    @PostMapping("/login")
    public LoginResponse login(@RequestBody LoginRequest req) {
        User user = userRepo.findByEmail(req.email())
            .orElseThrow(() -> new UnauthorizedException("Invalid credentials"));
        
        if (!passwordEncoder.matches(req.password(), user.getPasswordHash())) {
            throw new UnauthorizedException("Invalid credentials");
        }
        
        String accessToken = jwtService.generateAccessToken(user);
        String refreshToken = jwtService.generateRefreshToken(user);
        
        return new LoginResponse(accessToken, refreshToken, 900);
    }
    
    @PostMapping("/refresh")
    public RefreshResponse refresh(@RequestBody RefreshRequest req) {
        Claims claims = jwtService.verifyRefreshToken(req.refreshToken());
        User user = userRepo.findById(claims.get("userId", String.class))
            .orElseThrow(() -> new UnauthorizedException("Invalid token"));
        
        String newAccessToken = jwtService.generateAccessToken(user);
        return new RefreshResponse(newAccessToken, 900);
    }
    
    @PostMapping("/logout")
    public void logout(@RequestHeader("Authorization") String authHeader) {
        String token = authHeader.substring(7);
        blocklist.add(token);
    }
    
    @GetMapping("/.well-known/jwks.json")
    public Map<String, Object> jwks() {
        return jwtService.getPublicKeyJwks();
    }
}
```

### 8.2 `JwtService.java`

```java
package com.company.auth.service;

import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Service;

import java.security.PrivateKey;
import java.security.PublicKey;
import java.util.Date;

@Service
public class JwtService {
    
    private final PrivateKey privateKey;  // For signing
    private final PublicKey publicKey;    // For JWKS endpoint
    
    public JwtService(@Value("${jwt.private-key}") String privateKeyPem,
                      @Value("${jwt.public-key}") String publicKeyPem) {
        this.privateKey = loadPrivateKey(privateKeyPem);
        this.publicKey = loadPublicKey(publicKeyPem);
    }
    
    public String generateAccessToken(User user) {
        return Jwts.builder()
            .setIssuer("company-auth-service")
            .setAudience("company-frontend")
            .setSubject(user.getId())
            .claim("userId", user.getId())
            .claim("email", user.getEmail())
            .claim("roles", user.getRoles())
            .setIssuedAt(new Date())
            .setExpiration(new Date(System.currentTimeMillis() + 900_000))
            .signWith(privateKey, SignatureAlgorithm.RS256)
            .compact();
    }
    
    public String generateRefreshToken(User user) {
        return Jwts.builder()
            .setSubject(user.getId())
            .claim("userId", user.getId())
            .setExpiration(new Date(System.currentTimeMillis() + 2_592_000_000L))
            .signWith(privateKey, SignatureAlgorithm.RS256)
            .compact();
    }
    
    public Map<String, Object> getPublicKeyJwks() {
        // Convert public key to JWKS format for Next.js to fetch
        return JwksBuilder.fromPublicKey(publicKey);
    }
}
```

---

## 9. Environment Variables

### 9.1 Web app (`apps/web/.env`)

```env
AUTH_SERVICE_URL=http://auth-service.internal:8080
```

### 9.2 Checkout app (`apps/checkout/.env`)

```env
AUTH_SERVICE_URL=http://auth-service.internal:8080
```

### 9.3 Java auth service (`company-auth-service/.env`)

```env
JWT_PRIVATE_KEY=/path/to/private.pem
JWT_PUBLIC_KEY=/path/to/public.pem
DATABASE_URL=jdbc:postgresql://user-db.internal:5432/users
```

**Notice:** Next.js apps only know the URL of the auth service. They never see the private key. Only the Java service holds it.

---

## 10. Full Cross-App Auth Flow

**User logs in on `example.com/login`:**
1. Form submits → main web app's Server Action runs `loginWithBackend()`
2. `loginWithBackend()` (from `@company/auth-client`) calls Java service `POST /auth/login`
3. Java service verifies password against DB → generates JWT signed with private key → returns token
4. Next.js Server Action sets cookies on `.example.com`

**User navigates to `/checkout/cart` (crosses zone):**
5. Browser sends request to `example.com/checkout/cart` with cookies (auto — domain matches)
6. Main app's rewrite forwards to `checkout.example.com/checkout/cart` (cookies included)
7. Checkout app's middleware runs → calls `jwtVerify()` locally using cached public key (fast, no network call)
8. Middleware passes → page renders → `getSession()` returns user info
9. Page fetches cart from cart microservice, passing JWT in Authorization header
10. Cart microservice also verifies JWT with same public key → returns cart data

**Token expires mid-session:**
11. `jwtVerify()` throws `ERR_JWT_EXPIRED`
12. Auth client calls Java service `POST /auth/refresh` with refresh token
13. Java service issues new access token
14. Auth client sets new cookie → returns fresh session
15. User continues without noticing

**Logout:**
16. Any app calls `logout()` from `@company/auth-client`
17. Auth client calls Java service `POST /auth/logout` to blocklist the token
18. Auth client deletes cookies on `.example.com`
19. All apps immediately see user as logged out

---

## 11. Where Everything Lives

| Concern | Location |
|---|---|
| JWT signing private key | **Java auth service only** |
| JWT verification public key | Java service exposes JWKS endpoint; Next.js apps cache it |
| Password hashing/verification | Java auth service |
| User database queries | Java auth service |
| Token creation | Java auth service |
| Token verification | Next.js apps (local, using public key) — fast |
| Session revocation | Java auth service (blocklist) |
| Cookie handling | Next.js apps (`@company/auth-client` package) |
| Login UI | Main web app only |
| Auth logic reuse across Next.js apps | `packages/auth-client` (Turborepo workspace package) |

---

## 12. Deployment Model

**Java auth service (separate repo):**
- Deploys independently — own CI/CD, own release schedule
- Owned by backend/platform team
- Deployed to internal Kubernetes cluster or Spring Boot on EC2/etc.
- URL: `http://auth-service.internal:8080` (not publicly accessible)

**Turborepo (frontend monorepo):**
- Each Next.js app deploys independently
- `apps/web` → Vercel or self-hosted at `example.com`
- `apps/checkout` → Vercel or self-hosted at `checkout.example.com`
- `apps/admin` → Vercel or self-hosted at `admin.example.com`
- Shared packages (`@company/auth-client`, `@company/ui`) rebuilt automatically by Turborepo when apps rebuild
- Each app's deploy pulls latest published version of workspace packages at build time

---

## 13. Key Takeaways

1. **Java auth service is external** — lives in its own repo, own team, own deploy — Next.js just calls it
2. **Turborepo hosts the frontend only** — multiple Next.js apps + shared packages
3. **Auth logic is a shared package** (`@company/auth-client`) — every Next.js app imports the same functions
4. **JWT signing secret never leaves the Java service** — Next.js never touches it
5. **Cookies scoped to root domain** (`.example.com`) — all Next.js apps see them automatically
6. **Local JWT verification** uses Java's public key (JWKS endpoint) — fast, no network call per request
7. **Zone crossings work seamlessly** because the cookie is available to all apps via the shared root domain
8. **Refresh flow is transparent** — users never notice tokens expiring mid-session
9. **Central login page** — only main web app has `/login`; other apps redirect there
10. **Downstream microservices** also verify the same JWT — end-to-end auth propagation via `Authorization` header