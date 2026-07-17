# Enterprise Turborepo Monorepo — Complete Authentication Guide

> **For beginners to experts** — everything you need to understand JWT authentication, monorepo architecture, and how it all fits together.

---

# PART 1 — Understanding Authentication From Scratch

## What is Authentication?

**Authentication** = _"Who are you?"_ (proving your identity with email + password)

**Authorization** = _"What are you allowed to do?"_ (checking permissions after we know who you are)

Think of it like a nightclub:

| Nightclub                       | Auth System                                 |
| ------------------------------- | ------------------------------------------- |
| Showing your ID at the door     | **Login** (proving who you are)             |
| Getting a wristband             | **Token** (proof you were verified)         |
| Bouncer checking your wristband | **Verify** (checking token on each request) |
| Cutting off your wristband      | **Logout** (invalidating the token)         |

The whole flow in plain English:

```
1. User types email + password
2. Server checks: "Is this a real user with the right password?"
3. If yes, server gives user a special "pass" (called a JWT token)
4. User carries this pass with them (stored as a cookie)
5. Every time user asks server for something, they show the pass
6. Server checks the pass is valid → allows access
```

---

## What is a JWT (The "Pass")?

**JWT** = JSON Web Token — a string of characters that contains encoded user info and a cryptographic signature.

It looks like this:

```
eyJhbGciOiJSUzI1NiJ9.eyJ1c2VySWQiOiIxMjMiLCJlbWFpbCI6ImpvaG5AZXhhbXBsZS5jb20ifQ.aB3xC9dE7f...
```

It has **3 parts** separated by dots:

| Part          | What it contains                            | Example                                                               |
| ------------- | ------------------------------------------- | --------------------------------------------------------------------- |
| **Header**    | Algorithm used                              | `{ "alg": "RS256", "typ": "JWT" }`                                    |
| **Payload**   | User info                                   | `{ "userId": "123", "email": "john@example.com", "roles": ["user"] }` |
| **Signature** | Cryptographic proof it wasn't tampered with | `aB3xC9dE7f8gH2iJ4kL5m...`                                            |

> **Key idea:** Anyone can READ the payload (it is just base64-encoded), but only the server with the **secret private key** can CREATE a valid signature. If someone tries to fake a token, the signature will not match.

---

## The Two Keys — Private and Public

This is called **asymmetric cryptography** (public-key cryptography):

| Key             | Who Has It                          | What It Does                                 |
| --------------- | ----------------------------------- | -------------------------------------------- |
| **Private Key** | ONLY the auth server                | Creates valid JWT signatures                 |
| **Public Key**  | Shared with everyone (Next.js apps) | Verifies JWT signatures — cannot create them |

Real-world analogy:

```
Private key = your handwritten signature
             (only YOU can sign your name)

Public key  = a photo of your signature
             (anyone can compare a signed check against it,
              but they CANNOT sign new checks with it)
```

**Why Next.js apps can safely have the public key:**
Even if someone stole it, they could not create fake tokens without the private key.

---

## The Two Tokens — Access Token and Refresh Token

Real systems use **two tokens** together:

|               | Access Token                  | Refresh Token                                   |
| ------------- | ----------------------------- | ----------------------------------------------- |
| **Purpose**   | Used for every request        | Used only to get new access tokens              |
| **Lifespan**  | Short (15 minutes)            | Long (30 days)                                  |
| **Sent with** | Every API call                | Only when access token expires                  |
| **If stolen** | Damage limited — expires soon | Bigger problem — attacker gets long-term access |

**Why two tokens? Security balance:**

- Access tokens are used often → higher theft risk → keep them short-lived
- Refresh tokens are used rarely → lower theft risk → can be long-lived so users don't log in daily

---

## What is a Cookie?

A **cookie** is a small piece of data the browser stores and **automatically sends** with every request to the same domain.

When the auth service returns a JWT, Next.js tells the browser: _"Store this as a cookie."_ Every future request automatically attaches it.

### Important cookie settings:

| Setting                  | Meaning                                                                                                                                |
| ------------------------ | -------------------------------------------------------------------------------------------------------------------------------------- |
| `domain: '.example.com'` | Cookie is visible to `example.com` AND all subdomains (`checkout.example.com`, `admin.example.com`). The **leading dot** is the magic. |
| `httpOnly: true`         | JavaScript in the browser **CANNOT** read this cookie — only the server sees it. Prevents XSS attacks.                                 |
| `secure: true`           | Only sent over HTTPS, never HTTP. Prevents network eavesdropping.                                                                      |
| `sameSite: 'lax'`        | Cookie sent for normal navigation but not for suspicious cross-site requests. Prevents CSRF attacks.                                   |
| `path: '/'`              | Cookie valid for all paths                                                                                                             |
| `maxAge: 900`            | Cookie expires in 900 seconds (15 minutes)                                                                                             |

---

## How Credentials Are Actually Sent (Common Misconception)

> ❌ **Wrong:** Credentials are "encoded" before sending  
> ✅ **Correct:** Credentials are sent as plain text INSIDE an encrypted HTTPS tunnel

What actually happens:

```
1. Browser opens HTTPS connection (TLS handshake encrypts the connection)
2. Inside that encrypted tunnel, browser sends plain JSON:
   { "email": "john@example.com", "password": "hello123" }
3. Only the server can decrypt this because HTTPS is end-to-end encrypted
```

**Encoding** (like Base64) ≠ Security — anyone can decode it.  
**HTTPS encryption** = Security — requires a key to decrypt.

---

## How the Public Key Gets to Next.js (Separate From Login)

> ❌ **Wrong:** The public key is sent WITH the JWT during login  
> ✅ **Correct:** The public key is fetched SEPARATELY, once, before login even happens

The auth server exposes **two completely separate endpoints**:

```
Auth server:
├── POST /auth/login    → returns JWT only (no public key)
└── GET /.well-known/jwks.json  → returns public key (fetched separately, once)
```

**What JWKS looks like:**

```json
{
  "keys": [
    {
      "kty": "RSA",
      "use": "sig",
      "kid": "2024-01-key",
      "n": "xGOr-H0A-6_BOXMq83kU...",
      "e": "AQAB",
      "alg": "RS256"
    }
  ]
}
```

**Timeline:**

```
Next.js Server Starts Up
    ↓
Fetches public key from /.well-known/jwks.json
    ↓
Caches it in memory (reused forever, no network calls per request)
    ↓
User Logs In → JWT returned (no public key involved)
    ↓
User Visits Protected Page → Next.js uses CACHED public key to verify JWT
```

---

## Who Verifies the JWT? (Browser vs Server)

> ❌ **Wrong:** The browser (frontend) verifies the JWT  
> ✅ **Correct:** The Next.js SERVER verifies the JWT — the browser is just a courier

| Who                | Role                                                                               |
| ------------------ | ---------------------------------------------------------------------------------- |
| **Browser**        | Carries the cookie around, sends it with requests automatically — never reads it   |
| **Next.js Server** | Reads the cookie from incoming request, verifies signature using cached public key |

Why the browser cannot verify the JWT:

- JWT is in an `httpOnly` cookie → JavaScript **cannot** read it
- Only the Next.js server sees the cookie contents
- Only the Next.js server has the cached public key
- Browser is deliberately kept ignorant for security

**Analogy — movie theater:**

```
Private key   = the ticket PRINTER (only the ticket office has it)
JWT           = your actual TICKET (given to you at purchase)
Public key    = the barcode SCANNER (given to every usher on startup)

You (browser) = just carry the ticket, never scan it yourself
Usher (Next.js server) = has the scanner, verifies your ticket
```

---

## The Three Separate Flows

| Flow                                       | Uses Private Key? | Uses Public Key?    |
| ------------------------------------------ | ----------------- | ------------------- |
| Auth server signs a JWT (during login)     | ✅ Yes            | ❌ No               |
| Next.js verifies a JWT (during page loads) | ❌ No             | ✅ Yes              |
| Auth server exposes public key via JWKS    | ❌ No             | ✅ Yes (returns it) |

---

# PART 2 — Overall Architecture

## The Big Picture

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

- Java auth service lives in its **OWN** Git repo (not in the Turborepo)
- Turborepo contains only Next.js apps + shared frontend packages
- Java service owns password validation, JWT signing, session management
- Next.js apps are thin clients that call the Java service

### The three main players:

```
YOU (browser)
    ↓ visit websites, click login, add to cart

NEXT.JS APPS (three of them: main site, checkout, admin)
    ↓ check if user is logged in → ask the auth service

JAVA AUTH SERVICE (one central authority)
    ↓ check the database and issue tokens

DATABASE (stores real user accounts and hashed passwords)
```

---

## What is Turborepo?

**Turborepo** = a tool for managing multiple projects in one Git repo (called a "monorepo").

Instead of 3 separate Git repos for 3 Next.js apps, you keep them all in ONE repo:

```
company-frontend-monorepo/
├── apps/
│   ├── web/          ← Next.js app #1 (main site)
│   ├── checkout/     ← Next.js app #2 (checkout)
│   └── admin/        ← Next.js app #3 (admin)
├── packages/
│   ├── auth-client/  ← Shared code all 3 apps use
│   └── ui/           ← Shared React components
└── turbo.json
```

**Benefits:**

- Change shared code once → all apps get the update
- Easier consistency across apps
- One place to run tests, linting, builds
- Speeds up builds by caching (if `apps/web` didn't change, don't rebuild it)

> **What does `"workspace:*"` mean?**  
> `"@company/auth-client": "workspace:*"` means: _"Use the `auth-client` package from THIS monorepo, not from npm."_

---

# PART 3 — Project Structure

## Turborepo Structure

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

# PART 4 — Root Configuration Files

## `package.json` (root)

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

## `pnpm-workspace.yaml`

```yaml
packages:
  - 'apps/*'
  - 'packages/*'
```

## `turbo.json`

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

# PART 5 — Shared Auth Client Package

## `packages/auth-client/package.json`

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

## `packages/auth-client/src/index.ts`

```typescript
import { cookies } from 'next/headers';
import { jwtVerify, createRemoteJWKSet } from 'jose';

const AUTH_SERVICE_URL = process.env.AUTH_SERVICE_URL!;
// e.g., 'http://auth-service.internal:8080' (only reachable from Next.js server)

// ─────────────────────────────────────────────────────
// PHASE 0: Public key fetched ONCE from Java service on startup
// Cached in memory — no network call on every request
// ─────────────────────────────────────────────────────
const JWKS = createRemoteJWKSet(
  new URL(`${AUTH_SERVICE_URL}/auth/.well-known/jwks.json`)
);

// ─────────────────────────────────────────────────────
// PHASE 1: LOGIN — called from Server Action in main web app
// ─────────────────────────────────────────────────────
export async function loginWithBackend(email: string, password: string) {
  // Credentials sent over HTTPS (plain JSON inside encrypted tunnel)
  const response = await fetch(`${AUTH_SERVICE_URL}/auth/login`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ email, password })
  });

  if (!response.ok) {
    throw new Error('Invalid credentials');
  }

  // JWT returned — NO public key in this response
  const { accessToken, refreshToken, expiresIn } = await response.json();

  // Cookies scoped to ROOT domain so ALL Next.js apps can read them
  cookies().set('access_token', accessToken, {
    domain: '.example.com', // ← dot means ALL subdomains
    path: '/',
    httpOnly: true, // ← browser JS cannot read this
    secure: true, // ← HTTPS only
    sameSite: 'lax', // ← CSRF protection
    maxAge: expiresIn // ← 15 minutes
  });

  cookies().set('refresh_token', refreshToken, {
    domain: '.example.com',
    path: '/',
    httpOnly: true,
    secure: true,
    sameSite: 'lax',
    maxAge: 60 * 60 * 24 * 30 // ← 30 days
  });
}

// ─────────────────────────────────────────────────────
// PHASE 2: GET SESSION — used by ALL Next.js apps
// Next.js SERVER verifies JWT using cached public key
// Browser just carries the cookie — never reads it
// ─────────────────────────────────────────────────────
export async function getSession() {
  const accessToken = cookies().get('access_token')?.value;
  if (!accessToken) return null;

  try {
    // Uses CACHED public key — no network call here
    const { payload } = await jwtVerify(accessToken, JWKS, {
      issuer: 'company-auth-service',
      audience: 'company-frontend'
    });

    return {
      userId: payload.userId as string,
      email: payload.email as string,
      roles: payload.roles as string[]
    };
  } catch (error: any) {
    if (error.code === 'ERR_JWT_EXPIRED') {
      // PHASE 4: Token expired — silently refresh
      return await refreshSession();
    }
    return null;
  }
}

// ─────────────────────────────────────────────────────
// PHASE 4: REFRESH — silent token refresh (user doesn't notice)
// ─────────────────────────────────────────────────────
async function refreshSession() {
  const refreshToken = cookies().get('refresh_token')?.value;
  if (!refreshToken) return null;

  const response = await fetch(`${AUTH_SERVICE_URL}/auth/refresh`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ refreshToken })
  });

  if (!response.ok) return null;

  const { accessToken, expiresIn } = await response.json();

  // Set new access token cookie
  cookies().set('access_token', accessToken, {
    domain: '.example.com',
    path: '/',
    httpOnly: true,
    secure: true,
    sameSite: 'lax',
    maxAge: expiresIn
  });

  const { payload } = await jwtVerify(accessToken, JWKS);
  return {
    userId: payload.userId as string,
    email: payload.email as string,
    roles: payload.roles as string[]
  };
}

// ─────────────────────────────────────────────────────
// PHASE 5: LOGOUT — clear cookies from browser
// ─────────────────────────────────────────────────────
export async function logout() {
  const accessToken = cookies().get('access_token')?.value;

  if (accessToken) {
    // Tell Java service to blacklist this token
    await fetch(`${AUTH_SERVICE_URL}/auth/logout`, {
      method: 'POST',
      headers: { Authorization: `Bearer ${accessToken}` }
    });
  }

  // Delete cookies — maxAge: 0 removes them from browser
  cookies().delete({ name: 'access_token', domain: '.example.com', path: '/' });
  cookies().delete({
    name: 'refresh_token',
    domain: '.example.com',
    path: '/'
  });
}

// ─────────────────────────────────────────────────────
// HELPER: GET AUTH TOKEN — for calling other backend services
// ─────────────────────────────────────────────────────
export function getAuthToken() {
  return cookies().get('access_token')?.value;
}
```

---

# PART 6 — Main Web App (`apps/web/`)

## `apps/web/package.json`

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

## `apps/web/next.config.js` — Multi-Zone Rewrites

```javascript
// Multi-Zone: /checkout/* requests are forwarded to checkout app
// Cookie is included because domain matches
module.exports = {
  async rewrites() {
    return [
      {
        source: '/checkout/:path*',
        destination: 'https://checkout.example.com/checkout/:path*'
      }
    ];
  }
};
```

## `apps/web/app/login/page.tsx` — Login Page

```tsx
import { loginWithBackend } from '@company/auth-client';
import { redirect } from 'next/navigation';

export default function LoginPage() {
  // 'use server' = this function runs on Next.js server, NOT in browser
  // Password never touches the browser — server handles it directly
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
      <input name="email" type="email" placeholder="Email" required />
      <input name="password" type="password" placeholder="Password" required />
      <button type="submit">Log in</button>
    </form>
  );
}
```

## `apps/web/app/dashboard/page.tsx` — Protected Page

```tsx
import { getSession, getAuthToken } from '@company/auth-client';
import { redirect } from 'next/navigation';

export default async function DashboardPage() {
  // getSession() reads cookie → verifies JWT with cached public key → returns user
  const user = await getSession();
  if (!user) redirect('/login'); // No session → send to login

  // Pass JWT to other microservices via Authorization header
  const dashboardData = await fetch('http://dashboard-service.internal/data', {
    headers: { Authorization: `Bearer ${getAuthToken()}` },
    cache: 'no-store'
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

# PART 7 — Checkout App (`apps/checkout/`)

## `apps/checkout/package.json`

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

## `apps/checkout/next.config.js`

```javascript
module.exports = {
  basePath: '/checkout',
  assetPrefix: 'https://checkout.example.com'
};
```

## `apps/checkout/middleware.ts` — Route Guard

```typescript
import { NextResponse } from 'next/server';
import type { NextRequest } from 'next/server';
import { jwtVerify, createRemoteJWKSet } from 'jose';

const AUTH_SERVICE_URL = process.env.AUTH_SERVICE_URL!;

// Each Next.js app has its OWN cached copy of the public key
// Fetched independently on startup — no coordination needed
const JWKS = createRemoteJWKSet(
  new URL(`${AUTH_SERVICE_URL}/auth/.well-known/jwks.json`)
);

export async function middleware(request: NextRequest) {
  // Browser automatically sends the .example.com cookie here too
  // because checkout.example.com matches the .example.com domain
  const token = request.cookies.get('access_token')?.value;

  if (!token) {
    // No token → redirect to main app's login page
    return NextResponse.redirect(new URL('https://example.com/login'));
  }

  try {
    // Same public key, same verification — token was issued by same Java service
    await jwtVerify(token, JWKS);
    return NextResponse.next();
  } catch {
    return NextResponse.redirect(new URL('https://example.com/login'));
  }
}

export const config = {
  matcher: ['/checkout/:path*']
};
```

## `apps/checkout/app/checkout/cart/page.tsx`

```tsx
import { getSession, getAuthToken } from '@company/auth-client';
import { redirect } from 'next/navigation';

export default async function CartPage() {
  // PHASE 3: Multi-app navigation
  // Same getSession() function, same cookie, same verification
  // User is ALREADY authenticated — no second login needed
  const user = await getSession();

  if (!user) redirect('https://example.com/login');

  const cart = await fetch(`http://cart-service.internal/cart/${user.userId}`, {
    headers: { Authorization: `Bearer ${getAuthToken()}` },
    cache: 'no-store'
  }).then(r => r.json());

  return (
    <div>
      <h1>{user.email}'s Cart</h1>
      {cart.items.map(item => (
        <div key={item.id}>
          {item.name} — ${item.price}
        </div>
      ))}
    </div>
  );
}
```

---

# PART 8 — Admin App (`apps/admin/`)

## `apps/admin/app/users/page.tsx`

```tsx
import { getSession, getAuthToken } from '@company/auth-client';
import { redirect } from 'next/navigation';

export default async function UsersPage() {
  const user = await getSession();

  if (!user) redirect('https://example.com/login');

  // Role-based authorization — not just authentication
  // Auth = who are you? | Authorization = what can you do?
  if (!user.roles.includes('admin')) {
    return <div>Access denied — admins only</div>;
  }

  const users = await fetch('http://user-service.internal/users', {
    headers: { Authorization: `Bearer ${getAuthToken()}` }
  }).then(r => r.json());

  return (
    <div>
      <h1>User Management</h1>
      {users.map(u => (
        <div key={u.id}>{u.email}</div>
      ))}
    </div>
  );
}
```

---

# PART 9 — Java Auth Service (Separate Repo)

> This lives in a **completely separate Git repo**, maintained by the backend team. Included so you understand what issues tokens and why.

## `AuthController.java`

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

    // PHASE 1: Login — validates credentials, issues tokens
    @PostMapping("/login")
    public LoginResponse login(@RequestBody LoginRequest req) {
        // Step 1: Find user by email
        User user = userRepo.findByEmail(req.email())
            .orElseThrow(() -> new UnauthorizedException("Invalid credentials"));

        // Step 2: Compare password against stored HASH (plain password never stored)
        // BCrypt hashes "hello123" → "$2a$10$N9qo8uLOickgx2Z..."
        // passwordEncoder.matches() hashes the input and compares hashes
        if (!passwordEncoder.matches(req.password(), user.getPasswordHash())) {
            throw new UnauthorizedException("Invalid credentials");
        }

        // Step 3: Create tokens signed with PRIVATE key
        String accessToken = jwtService.generateAccessToken(user);
        String refreshToken = jwtService.generateRefreshToken(user);

        // Return ONLY the tokens — no public key in this response
        return new LoginResponse(accessToken, refreshToken, 900);
    }

    // PHASE 4: Refresh — issues new access token using refresh token
    @PostMapping("/refresh")
    public RefreshResponse refresh(@RequestBody RefreshRequest req) {
        Claims claims = jwtService.verifyRefreshToken(req.refreshToken());
        User user = userRepo.findById(claims.get("userId", String.class))
            .orElseThrow(() -> new UnauthorizedException("Invalid token"));

        String newAccessToken = jwtService.generateAccessToken(user);
        return new RefreshResponse(newAccessToken, 900);
    }

    // PHASE 5: Logout — blacklists the token
    @PostMapping("/logout")
    public void logout(@RequestHeader("Authorization") String authHeader) {
        String token = authHeader.substring(7); // Remove "Bearer "
        blocklist.add(token); // Token is now invalid even if not expired
    }

    // PHASE 0: JWKS endpoint — public key available to ALL Next.js apps
    // Anyone can call this — it's intentionally public
    @GetMapping("/.well-known/jwks.json")
    public Map<String, Object> jwks() {
        return jwtService.getPublicKeyJwks();
    }
}
```

## `JwtService.java`

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

    private final PrivateKey privateKey;  // ⚠️ NEVER leaves this service
    private final PublicKey publicKey;    // Shared via JWKS endpoint

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
            .setExpiration(new Date(System.currentTimeMillis() + 900_000)) // 15 min
            .signWith(privateKey, SignatureAlgorithm.RS256) // ← signed with PRIVATE key
            .compact();
    }

    public String generateRefreshToken(User user) {
        return Jwts.builder()
            .setSubject(user.getId())
            .claim("userId", user.getId())
            .setExpiration(new Date(System.currentTimeMillis() + 2_592_000_000L)) // 30 days
            .signWith(privateKey, SignatureAlgorithm.RS256)
            .compact();
    }

    // Returns public key in JWKS format for Next.js to fetch and cache
    public Map<String, Object> getPublicKeyJwks() {
        return JwksBuilder.fromPublicKey(publicKey);
    }
}
```

---

# PART 10 — Environment Variables

## Web App (`apps/web/.env`)

```env
AUTH_SERVICE_URL=http://auth-service.internal:8080
```

## Checkout App (`apps/checkout/.env`)

```env
AUTH_SERVICE_URL=http://auth-service.internal:8080
```

## Java Auth Service (separate repo `.env`)

```env
JWT_PRIVATE_KEY=/path/to/private.pem
JWT_PUBLIC_KEY=/path/to/public.pem
DATABASE_URL=jdbc:postgresql://user-db.internal:5432/users
```

> **Critical security note:** Next.js apps only know the URL of the auth service. They **never** see the private key. Only the Java service holds it.

---

# PART 11 — All Authentication Phases Explained

## Phase 0: Server Startup — Fetching and Caching the Public Key

**When:** Next.js app starts, before any users interact with it

```
Next.js Server Starts
    ↓
jose library sees JWKS URL in createRemoteJWKSet()
    ↓
HTTP GET https://auth-service.internal/.well-known/jwks.json
    ↓
Auth server returns public key JSON
    ↓
Public key cached in Next.js memory
    ↓
DONE — no more fetching until key rotation
```

**This happens ONCE per app startup. All subsequent verifications reuse the cached key.**

---

## Phase 1: User Logs In

**When:** User enters email + password and clicks "Log in"

```
User types: john@example.com / hello123
    ↓
Browser opens HTTPS connection (TLS encrypts the tunnel)
    ↓
Browser sends plain JSON INSIDE the encrypted tunnel:
{ "email": "john@example.com", "password": "hello123" }
    ↓
Next.js Server Action receives it ('use server' — runs on server, not browser)
    ↓
Server Action calls loginWithBackend(email, password)
    ↓
Next.js calls: POST http://auth-service.internal:8080/auth/login
    ↓
Java: find user by email in DB
    ↓
Java: passwordEncoder.matches("hello123", storedHash) → true
    ↓
Java: sign JWT with PRIVATE key (private key never leaves Java service)
    ↓
Java returns: { accessToken: "eyJ...", refreshToken: "eyJ..." }
    ↓
Next.js sets cookies on .example.com (httpOnly, secure, sameSite)
    ↓
Browser stores cookies — sends them automatically with future requests
    ↓
User redirected to /dashboard
```

**What the JWT looks like decoded:**

```json
{
  "header": { "alg": "RS256", "typ": "JWT" },
  "payload": {
    "userId": "usr_12345",
    "email": "john@example.com",
    "roles": ["user", "subscriber"],
    "iat": 1705419000,
    "exp": 1705419900,
    "iss": "company-auth-service",
    "aud": "company-frontend"
  },
  "signature": "aB3xC9dE7f8gH2iJ4kL5m..."
}
```

---

## Phase 2: Accessing a Protected Page

**When:** User navigates to `/dashboard` or any protected route

```
User clicks link to /dashboard
    ↓
Browser automatically attaches cookie:
GET /dashboard
Cookie: access_token=eyJ...    ← sent automatically!
    ↓
Next.js server reads the cookie
    ↓
Next.js calls jwtVerify(token, JWKS)
    ↓
jose library uses CACHED public key (no network call — milliseconds)
    ↓
Signature valid? ✅
    ↓
Extract payload: { userId, email, roles }
    ↓
Page renders: "Welcome, john@example.com"
```

---

## Phase 3: Multi-App Navigation (Cookie Shared Across Subdomains)

**When:** User navigates from `example.com` to `checkout.example.com`

```
User on example.com/dashboard
User clicks "Go to checkout"
    ↓
Browser navigates to example.com/checkout/cart
    ↓
Main app's rewrite forwards request to checkout.example.com/checkout/cart
Cookie is INCLUDED in forwarded request (domain matches .example.com)
    ↓
Checkout app's middleware intercepts
    ↓
Reads SAME access_token cookie (same domain .example.com)
    ↓
Checkout app's CACHED public key verifies the token (same Java service issued it)
    ↓
Token valid ✅ — user is authenticated
    ↓
CartPage loads, user sees their cart — NO second login required
```

**Why it works:**

```
Cookie set with domain: .example.com
                         ↑
              The leading dot means:
              ├── example.com          ✅ sees the cookie
              ├── checkout.example.com ✅ sees the cookie
              ├── admin.example.com    ✅ sees the cookie
              └── api.example.com      ✅ sees the cookie
```

---

## Phase 4: Token Expires — Silent Refresh

**When:** Access token expires (15 minutes after login) and user makes a request

```
User still browsing (token silently expired)
    ↓
User clicks a link → request sent with expired token
    ↓
Next.js middleware: jwtVerify() throws ERR_JWT_EXPIRED
    ↓
Middleware detects expiry → calls refreshSession()
    ↓
refreshSession() reads refresh_token cookie
    ↓
POST http://auth-service.internal:8080/auth/refresh
{ "refreshToken": "eyJ..." }
    ↓
Java verifies refresh token (also signed with private key)
    ↓
Java generates NEW access token signed with private key
    ↓
Next.js receives new access token
    ↓
Next.js sets new access_token cookie (maxAge: 15 minutes again)
    ↓
Request continues — user doesn't notice anything
```

**Timeline example:**

```
14:00:00 — User logs in
           access_token  expires: 14:15:00
           refresh_token expires: 30 days later

14:14:55 — User browsing — token still valid ✅

14:15:05 — User clicks link
           Middleware: ERR_JWT_EXPIRED
           Silent refresh → new access_token expires: 14:30:05
           User sees: nothing (seamless)

14:30:10 — Same silent refresh again
           access_token expires: 14:45:10

Day 30   — refresh_token expires
           Java refuses to refresh
           User redirected to login page
           User must enter credentials again
```

---

## Phase 5: Logout — Clearing Cookies

**When:** User clicks "Log Out"

```
User clicks "Log Out"
    ↓
logout() function called
    ↓
POST /auth/logout → Java service adds token to blacklist
(even if token not expired, it's now invalid server-side)
    ↓
Next.js deletes access_token cookie (maxAge: 0)
Next.js deletes refresh_token cookie (maxAge: 0)
    ↓
Browser removes cookies for .example.com
    ↓
ALL subdomains immediately lose auth:
├── example.com → no cookie → redirect to login
├── checkout.example.com → no cookie → redirect to login
└── admin.example.com → no cookie → redirect to login
    ↓
User sees login page
```

---

## Phase 6: Logged-Out User Tries to Access Protected Page

**When:** After logout, user tries to visit `/dashboard`

```
User tries: example.com/dashboard
    ↓
Browser sends request — NO cookie (was deleted)
    ↓
Next.js middleware: cookies().get('access_token') → undefined
    ↓
Middleware: no token → redirect to /login
    ↓
User sees login page
```

---

# PART 12 — Complete Visual Flow

```
USER JOURNEY (from first visit to logout):

╔══════════════════════════════════════════════════════╗
║  PHASE 0: STARTUP (before any user visits)           ║
╚══════════════════════════════════════════════════════╝

Next.js boots up
    → Fetches public key from /.well-known/jwks.json
    → Caches in memory
    → Ready

╔══════════════════════════════════════════════════════╗
║  PHASE 1: LOGIN                                      ║
╚══════════════════════════════════════════════════════╝

Browser ──(HTTPS encrypted)──→ Next.js Server
         { email, password }
                                    ↓
                              loginWithBackend()
                                    ↓
Next.js ──────────────────────→ Java Auth Service
        POST /auth/login             ↓
        { email, password }    Check DB + hash
                                    ↓
Next.js ←──────────────────── Java Auth Service
        { accessToken,         Sign with PRIVATE KEY
          refreshToken }
                ↓
        Set cookies on .example.com
        (httpOnly, secure, sameSite)
                ↓
        Redirect to /dashboard

╔══════════════════════════════════════════════════════╗
║  PHASE 2: ACCESSING PROTECTED PAGES                  ║
╚══════════════════════════════════════════════════════╝

Browser ──(with cookie)──→ Next.js Server
  GET /dashboard               ↓
  Cookie: access_token=eyJ...  ↓
                         jwtVerify(token, CACHED_PUBLIC_KEY)
                               ↓ (milliseconds, no network)
                         ✅ Valid
                               ↓
                         Extract: { userId, email, roles }
                               ↓
                         Render page

╔══════════════════════════════════════════════════════╗
║  PHASE 3: MULTI-APP (same cookie, different subdomain)║
╚══════════════════════════════════════════════════════╝

Browser ──→ example.com/checkout/cart
                ↓ (rewrite)
            checkout.example.com/checkout/cart
                ↓ (same cookie, domain matches)
            Checkout middleware verifies JWT
                ↓ (same public key, same result)
            ✅ Authenticated — no second login

╔══════════════════════════════════════════════════════╗
║  PHASE 4: TOKEN EXPIRES (silent refresh)             ║
╚══════════════════════════════════════════════════════╝

Browser ──→ Request with expired access_token
                ↓
            ERR_JWT_EXPIRED
                ↓
            POST /auth/refresh with refresh_token
                ↓
            Java issues new access_token
                ↓
            New cookie set
                ↓
            ✅ Request continues — user doesn't notice

╔══════════════════════════════════════════════════════╗
║  PHASE 5: LOGOUT                                     ║
╚══════════════════════════════════════════════════════╝

Browser clicks "Logout"
    ↓
Java blacklists token
    ↓
Cookies deleted (maxAge: 0) on .example.com
    ↓
ALL subdomains lose auth immediately
    ↓
User sees login page
```

---

# PART 13 — Dependencies Explained

## Frontend (Next.js)

| Package             | Purpose                                                                                                        |
| ------------------- | -------------------------------------------------------------------------------------------------------------- |
| `next ^15.0.0`      | React framework — App Router, Server Components, Server Actions, Middleware                                    |
| `react ^19.0.0`     | React library — Server Components stabilized in v19                                                            |
| `jose ^5.0.0`       | JWT operations — `jwtVerify()`, `createRemoteJWKSet()`. Works in Edge Runtime (needed for Next.js middleware). |
| `turbo ^2.0.0`      | Monorepo build tool — caching, task coordination                                                               |
| `typescript ^5.3.0` | Type safety — all the `: string`, `: FormData` syntax                                                          |

## Java (Spring Boot)

| Package                    | Purpose                                                                                  |
| -------------------------- | ---------------------------------------------------------------------------------------- |
| `Spring Boot`              | HTTP routing (`@RestController`), dependency injection (`@Autowired`), config (`@Value`) |
| `jjwt (io.jsonwebtoken)`   | JWT creation and verification on Java side                                               |
| `PasswordEncoder (BCrypt)` | Hashes passwords — turns "hello123" into irreversible hash                               |
| `Spring Security`          | Security primitives — authentication filters, password encoding                          |

---

# PART 14 — Where Everything Lives

| Concern                         | Location                                                |
| ------------------------------- | ------------------------------------------------------- |
| JWT **signing** private key     | **Java auth service ONLY** — never exposed              |
| JWT **verification** public key | Java service JWKS endpoint → cached in each Next.js app |
| Password hashing/verification   | Java auth service                                       |
| User database queries           | Java auth service                                       |
| Token creation                  | Java auth service                                       |
| Token verification              | Next.js apps (local, using cached public key) — fast    |
| Session revocation / blacklist  | Java auth service                                       |
| Cookie handling                 | Next.js apps (`@company/auth-client` package)           |
| Login UI                        | Main web app only (`apps/web`)                          |
| Auth logic reuse across apps    | `packages/auth-client` (Turborepo shared package)       |

---

# PART 15 — Deployment Model

## Java Auth Service (separate repo)

```
company-auth-service/ (Git repo)
    ↓ own CI/CD pipeline
    ↓ owned by backend/platform team
Kubernetes cluster (internal)
    ↓ URL: http://auth-service.internal:8080
    ↓ NOT publicly accessible (only Next.js servers can reach it)
```

## Turborepo (frontend monorepo)

```
company-frontend-monorepo/ (Git repo)
    ↓
├── apps/web      → Vercel / self-hosted at example.com
├── apps/checkout → Vercel / self-hosted at checkout.example.com
└── apps/admin    → Vercel / self-hosted at admin.example.com

Shared packages (@company/auth-client, @company/ui)
→ rebuilt automatically by Turborepo when apps rebuild
→ each app's deploy gets latest workspace package at build time
```

---

# PART 16 — Security Summary

| Layer                        | Protection                                | Guards Against                             |
| ---------------------------- | ----------------------------------------- | ------------------------------------------ |
| **Private key**              | Kept only in Java auth service            | Cannot forge JWTs without it               |
| **HTTPS**                    | Encrypts credentials in transit           | Network eavesdropping                      |
| **httpOnly cookie**          | Browser JS cannot read the token          | XSS attacks stealing tokens                |
| **sameSite: lax**            | Controls cross-site cookie sending        | CSRF attacks                               |
| **Access token expiration**  | 15 minutes                                | Stolen token has limited damage window     |
| **Refresh token expiration** | 30 days                                   | User must re-authenticate eventually       |
| **Token blacklist**          | Java service tracks logged-out tokens     | Logout is immediate, not just cookie-based |
| **Root domain cookie**       | `.example.com` — shared across subdomains | Single login for whole platform            |

### What would be a security problem vs. what is fine:

| Scenario                    | Verdict                                  |
| --------------------------- | ---------------------------------------- |
| **Public key is public**    | ✅ Fine — by design, anyone can have it  |
| **Private key exposed**     | ❌ Critical — anyone can forge any JWT   |
| **Token sent over HTTP**    | ❌ Critical — eavesdropper can steal it  |
| **Token in localStorage**   | ❌ High — XSS can steal it               |
| **Token has no expiration** | ❌ Medium — stolen token works forever   |
| **Cookies on .example.com** | ✅ Fine — intentional for multi-app auth |

---

# PART 17 — Key Takeaways

1. **Authentication** = who are you | **Authorization** = what can you do
2. **JWT** = a signed token with user info — signature proves it wasn't tampered
3. **Private key** = only auth server has it — creates tokens
4. **Public key** = shared with Next.js apps — verifies tokens — cannot create them
5. **Public key fetched ONCE** at startup via JWKS endpoint — cached — reused
6. **Browser never verifies** — the Next.js SERVER verifies using cached public key
7. **Credentials sent as plain JSON** inside HTTPS-encrypted tunnel — not "encoded"
8. **Two tokens** — short access token (15 min) + long refresh token (30 days) = security balance
9. **httpOnly cookie** — browser carries it, cannot read it — only server sees it
10. **Root domain cookie** (`.example.com`) — one login works for all subdomains
11. **Silent refresh** — user never sees token expiry mid-session
12. **Single logout** — deleting root domain cookie logs out of ALL apps instantly
13. **Java service is external** — its own repo, own team, own deploy — Next.js just calls it
14. **Turborepo** — one Git repo for all Next.js apps + shared auth logic package
15. **Shared package** (`@company/auth-client`) — change auth logic once → all apps updated

---

## Summary

This complete guide covers:

- Authentication fundamentals (who, what, why)
- JWT structure and cryptography
- Public/private key asymmetric cryptography
- Access tokens vs refresh tokens
- Cookies and their security properties
- Turborepo monorepo architecture
- Project structure and file organization
- Shared auth client package implementation
- Multi-app login flow
- All 6 authentication phases
- Java backend auth service
- Environment configuration
- Security best practices
- Deployment models
- Common misconceptions and corrections

Use this as a reference for understanding enterprise authentication systems with modern Next.js monorepos and external Java auth services.
