# React.js Complete Interview Guide
## Category 19: Authentication (Enterprise Level) + Category 20: WebSocket & Real-Time

> These categories are missing from most React interview guides.
> Asked in almost every senior / enterprise React interview.
> Classified by frequency: 🔴 Must-know · 🟡 Should-know · 🟢 Good-to-know

---

# Table of Contents

- [Category 19: Authentication](#category-19-authentication)
  - [The 5 Points That Show Senior-Level Thinking — Auth](#the-5-points-that-show-senior-level-thinking--auth)
  - [19.1 What is JWT and how does it work?](#191-what-is-jwt-and-how-does-it-work)
  - [19.2 Token storage — memory vs localStorage vs httpOnly cookie](#192-token-storage--memory-vs-localstorage-vs-httponly-cookie)
  - [19.3 AuthContext — storing user and role globally](#193-authcontext--storing-user-and-role-globally)
  - [19.4 Axios interceptors — attaching token and silent refresh](#194-axios-interceptors--attaching-token-and-silent-refresh)
  - [19.5 PrivateRoute — protecting pages](#195-privateroute--protecting-pages)
  - [19.6 RBAC — role-based access control](#196-rbac--role-based-access-control)
  - [19.7 Login and logout flow end to end](#197-login-and-logout-flow-end-to-end)
  - [19.8 Handling page refresh — restoring auth state](#198-handling-page-refresh--restoring-auth-state)
  - [19.9 Multi-tab sync with BroadcastChannel](#199-multi-tab-sync-with-broadcastchannel)
  - [19.10 Auth providers — build vs buy](#1910-auth-providers--build-vs-buy)
  - [Top Interview Questions — Auth](#top-interview-questions--auth)
  - [Quick Revision — Category 19 One-Liners](#quick-revision--category-19-one-liners)
- [Category 20: WebSocket & Real-Time Communication](#category-20-websocket--real-time-communication)
  - [The 5 Points That Show Senior-Level Thinking — WebSocket](#the-5-points-that-show-senior-level-thinking--websocket)
  - [20.1 REST pull vs WebSocket push — when to use which](#201-rest-pull-vs-websocket-push--when-to-use-which)
  - [20.2 Native WebSocket API in React](#202-native-websocket-api-in-react)
  - [20.3 Socket.IO — why it is preferred in enterprise](#203-socketio--why-it-is-preferred-in-enterprise)
  - [20.4 Custom useSocket hook](#204-custom-usesocket-hook)
  - [20.5 TanStack Query + WebSocket — the hybrid pattern](#205-tanstack-query--websocket--the-hybrid-pattern)
  - [20.6 Zustand + WebSocket — storing live UI state](#206-zustand--websocket--storing-live-ui-state)
  - [20.7 Polling vs WebSocket vs SSE — comparison](#207-polling-vs-websocket-vs-sse--comparison)
  - [Top Interview Questions — WebSocket](#top-interview-questions--websocket)
  - [Quick Revision — Category 20 One-Liners](#quick-revision--category-20-one-liners)

---

# Category 19: Authentication

---

## The 5 Points That Show Senior-Level Thinking — Auth

> When an interviewer asks **"How did you implement authentication in your enterprise app?"**
> these are the 5 points that separate a senior answer from a junior answer.
> A junior says "we stored the token in localStorage and passed it in every fetch."
> A senior covers all 5 of these.

---

### Point 1 · We used JWT — access token in memory, refresh token in httpOnly cookie (not localStorage)

**What juniors say:**
> "We stored the JWT token in localStorage."

**What seniors say:**
> "We used JWT-based authentication. The server returns two tokens on login — a
> short-lived access token (15 minutes) and a long-lived refresh token (7 days).
> The access token we store in memory only as a plain JavaScript variable — never
> in localStorage. The refresh token the server sets as an httpOnly cookie.
> We never see or touch the refresh token on the frontend."

**Why this answer impresses the interviewer:** You understand XSS security. Most
developers store tokens in localStorage by default. Knowing why that is wrong and
what the secure alternative is shows you have thought about security at the architecture level.

---

### Point 2 · Axios interceptor attaches token to every request and handles 401 with silent refresh

**What juniors say:**
> "We passed the token manually in the headers of each fetch call."

**What seniors say:**
> "We configured an Axios request interceptor that reads the access token from memory
> and adds the Authorization Bearer header to every outgoing request automatically.
> No component ever manually adds the token. We also have a response interceptor that
> catches 401 responses — when a token expires the interceptor silently calls the
> refresh endpoint, gets a new access token, updates memory, and retries the original
> request. The component gets its data and never knows the token had expired."

**Why this answer impresses the interviewer:** You describe a clean separation of concerns —
components focus on business logic, the interceptor handles auth transparently. You also
mention the failed-request queue, which shows you have handled the edge case of multiple
simultaneous 401s.

---

### Point 3 · AuthContext wraps the app — user object and role available everywhere via useContext

**What juniors say:**
> "We passed the user as props down to each component that needed it."

**What seniors say:**
> "After login we store the user object — name, email, role, and permissions — in
> AuthContext. This wraps the entire app so any component can call useContext and
> get the current user without prop drilling. We chose Context over Zustand here
> because the user object rarely changes — only on login, logout, or profile update —
> so the re-render cost of Context is acceptable."

**Why this answer impresses the interviewer:** You explain WHY you chose Context over
Zustand for this specific case — showing you understand the trade-offs of each tool,
not just how to use them.

---

### Point 4 · PrivateRoute protects every page — redirects to /login if not authenticated

**What juniors say:**
> "We checked if the user was logged in inside each page component."

**What seniors say:**
> "We have a single PrivateRoute wrapper component that checks isLoggedIn from
> AuthContext before rendering any protected page. If not authenticated it redirects
> to /login. We also check isLoading — on every app load there is a moment while
> we are verifying the httpOnly cookie. Without the isLoading check, every logged-in
> user would see a flash of the login page before the auth check completes.
> For role-based routes, PrivateRoute accepts an allowedRoles prop and redirects
> to /403 if the user's role does not qualify."

**Why this answer impresses the interviewer:** You mention the isLoading flash — a subtle
but real UX problem that most candidates miss. Mentioning it shows you have actually
built and used this pattern in production, not just read about it.

---

### Point 5 · RBAC — JWT carries role + permissions, UI and routes both check it

**What juniors say:**
> "We checked if the user was admin before showing the button."

**What seniors say:**
> "The JWT payload carries both a role field — admin, manager, or user — and a
> permissions array with granular actions like read:orders or delete:orders.
> At the route level, PrivateRoute uses the role to block entire pages. At the
> UI level we have a Can component that wraps any element and conditionally renders
> based on role or permission. The most important thing — we never rely on frontend
> checks alone for security. The backend validates the JWT and checks permissions
> on every single API request. The frontend checks are purely for user experience."

**Why this answer impresses the interviewer:** The last sentence — "frontend is UX only,
real security is always server-side" — is exactly what senior developers say. Most
junior developers think hiding a button is security. Knowing the distinction shows
real security thinking.

---

### 🔴 19.1 What is JWT and how does it work?

**Explanation:** JWT (JSON Web Token) is a compact, self-contained token the server gives the client after login. It encodes user data (id, role, permissions) inside itself — the server does not need to look up a session in the database on every request. It just verifies the token's signature.

**A JWT has 3 parts separated by dots:**

```
eyJhbGciOiJIUzI1NiJ9   ← Header  (algorithm used to sign)
.eyJ1c2VySWQiOiIxMjMi} ← Payload (your data — userId, role, exp)
.SflKxwRJSMeKKF2QT4fw  ← Signature (proves it wasn't tampered with)
```

**The payload — what your frontend actually uses:**

```ts
// {
//   userId:      "abc123",
//   email:       "john@company.com",
//   role:        "admin",
//   permissions: ["read:orders", "write:orders", "delete:orders"],
//   iat:         1700000000,
//   exp:         1700000900    ← expires 15 minutes after issue
// }
```

**Two tokens — access token and refresh token:**

```
Access token:   Short-lived (15 min) · contains userId/role/permissions · stored in MEMORY
Refresh token:  Long-lived (7 days) · reference ID only · stored in httpOnly COOKIE
```

**Why two tokens instead of one long-lived token?**

```
One long-lived token: stolen → attacker has access for months
Two tokens:
  Access token stolen  → useless in 15 minutes
  Refresh token        → httpOnly cookie, JS cannot steal it
  Server can invalidate refresh token in DB on logout
```

**Interview Q: What is a JWT and what does it contain?**
**A:** JWT is a self-contained token with three base64-encoded parts — header (signing algorithm), payload (user data: id, role, permissions, expiry), and signature (cryptographic proof). The server signs it with a secret key. The frontend sends it with every request. The server verifies the signature — no database lookup needed.

**Common Mistake:** Thinking JWT is encrypted. It is NOT — the payload is only base64 encoded, anyone can decode it. Never put sensitive data (passwords, credit cards) in a JWT.

**One-liner:** JWT = 3-part base64 token (header.payload.signature). Contains user data. Server signs it. Frontend sends it. Not encrypted — just signed.

---

### 🔴 19.2 Token storage — memory vs localStorage vs httpOnly cookie

**Explanation:** Where you store the token is a critical security decision. Most developers default to localStorage — this is wrong and dangerous.

| Storage | Who can read it | XSS safe | CSRF safe | Survives refresh | Use for |
|---------|----------------|----------|-----------|-----------------|---------|
| localStorage | Any JS on the page | ❌ No | ✅ Yes | ✅ Yes | ❌ Never tokens |
| sessionStorage | Any JS on the page | ❌ No | ✅ Yes | ❌ No | ❌ Never tokens |
| Memory (JS var) | Only your own code | ✅ Yes | ✅ Yes | ❌ No | ✅ Access token |
| httpOnly cookie | Browser only — JS cannot read | ✅ Yes | ⚠️ Needs SameSite | ✅ Yes | ✅ Refresh token |

```ts
// src/shared/lib/tokenStore.ts
// Access token lives here — NOT in localStorage

let accessToken: string | null = null

export function setAccessToken(token: string) { accessToken = token }
export function getAccessToken(): string | null { return accessToken }
export function clearAccessToken() { accessToken = null }
```

**Cookie flags the server must set:**
```
HttpOnly      → JS cannot read the cookie
Secure        → only sent over HTTPS
SameSite=Strict → CSRF protection — only sent for same-origin requests
```

**Interview Q: Why should you never store tokens in localStorage?**
**A:** localStorage is accessible to any JavaScript on your page — your own code, third-party scripts, browser extensions, and injected malicious code. An XSS attack can steal tokens from localStorage with a single line. An httpOnly cookie cannot be read by JavaScript at all.

**Common Mistake:** Storing the refresh token in localStorage "for convenience." This defeats the entire purpose — the refresh token is the crown jewel.

**One-liner:** localStorage = any JS can steal it. Memory = only your code, gone on refresh. httpOnly cookie = JS cannot touch it, browser sends automatically. Access token → memory. Refresh token → httpOnly cookie.

---

### 🔴 19.3 AuthContext — storing user and role globally

**Explanation:** After login, the user object needs to be available to every component. Context is the right tool because the user rarely changes.

```tsx
// src/features/auth/model/AuthContext.tsx

interface User {
  id: string
  name: string
  email: string
  role: 'admin' | 'manager' | 'user'
  permissions: string[]
}

interface AuthContextType {
  user: User | null
  isLoggedIn: boolean
  isLoading: boolean
  login: (email: string, password: string) => Promise<void>
  logout: () => Promise<void>
}

const AuthContext = createContext<AuthContextType | null>(null)

export function AuthProvider({ children }: { children: ReactNode }) {
  const [user, setUser] = useState<User | null>(null)
  const [isLoading, setIsLoading] = useState(true)

  useEffect(() => {
    async function checkAuth() {
      try {
        const res = await fetch('/api/auth/me', { credentials: 'include' })
        if (res.ok) {
          const data = await res.json()
          setAccessToken(data.accessToken)
          setUser(data.user)
        }
      } catch {
        setUser(null)
      } finally {
        setIsLoading(false)
      }
    }
    checkAuth()
  }, [])

  async function login(email: string, password: string) {
    const res = await fetch('/api/auth/login', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ email, password }),
      credentials: 'include'
    })
    if (!res.ok) throw new Error('Invalid credentials')
    const data = await res.json()
    setAccessToken(data.accessToken)
    setUser(data.user)
  }

  async function logout() {
    await fetch('/api/auth/logout', { method: 'POST', credentials: 'include' })
    setUser(null)
    clearAccessToken()
    queryClient.clear()  // wipe TanStack Query cache
  }

  return (
    <AuthContext.Provider value={{ user, isLoggedIn: !!user, isLoading, login, logout }}>
      {children}
    </AuthContext.Provider>
  )
}

export function useAuth() {
  const context = useContext(AuthContext)
  if (!context) throw new Error('useAuth must be inside AuthProvider')
  return context
}
```

**Interview Q: Why use Context for auth user instead of Zustand?**
**A:** The user object rarely changes — only on login, logout, or profile update. Context is perfect for slow-changing global values. Zustand is better when state changes frequently (cart, modals) because Context re-renders all consumers on every change. For auth that re-render happens almost never.

**Common Mistake:** Not handling `isLoading`. Without it, logged-in users briefly see the login page on every app load before the auth check completes.

**One-liner:** AuthContext stores user, isLoggedIn, login(), logout(). `useAuth()` reads it anywhere. `isLoading` prevents the login page flash on startup.

---

### 🔴 19.4 Axios interceptors — attaching token and silent refresh

**Explanation:** Two interceptors handle the entire token lifecycle. Components never manually add tokens or handle 401 errors.

```ts
// src/shared/lib/axios.ts
export const apiClient = axios.create({
  baseURL: import.meta.env.VITE_API_URL,
  withCredentials: true,
})

// INTERCEPTOR 1 — REQUEST: attach token automatically
apiClient.interceptors.request.use((config) => {
  const token = getAccessToken()
  if (token) config.headers.Authorization = `Bearer ${token}`
  return config
})

// INTERCEPTOR 2 — RESPONSE: catch 401, silent refresh, retry
let isRefreshing = false
let failedQueue: Array<{ resolve: (t: string) => void; reject: (e: unknown) => void }> = []

function processQueue(error: unknown, token: string | null = null) {
  failedQueue.forEach(({ resolve, reject }) => error ? reject(error) : resolve(token!))
  failedQueue = []
}

apiClient.interceptors.response.use(
  (response) => response,
  async (error) => {
    const originalRequest = error.config
    if (error.response?.status === 401 && !originalRequest._retry) {
      if (isRefreshing) {
        return new Promise((resolve, reject) => {
          failedQueue.push({ resolve, reject })
        }).then((token) => {
          originalRequest.headers.Authorization = `Bearer ${token}`
          return apiClient(originalRequest)
        })
      }
      originalRequest._retry = true
      isRefreshing = true
      try {
        const res = await axios.post('/api/auth/refresh', {}, { withCredentials: true })
        const newToken = res.data.accessToken
        setAccessToken(newToken)
        processQueue(null, newToken)
        originalRequest.headers.Authorization = `Bearer ${newToken}`
        return apiClient(originalRequest)
      } catch (refreshError) {
        processQueue(refreshError, null)
        clearAccessToken()
        window.location.href = '/login'
        return Promise.reject(refreshError)
      } finally {
        isRefreshing = false
      }
    }
    return Promise.reject(error)
  }
)
```

**The silent refresh flow:**
```
1.  Component calls:  GET /api/orders
2.  Interceptor 1:    Adds Authorization: Bearer <expired-token>
3.  Server:           Returns 401
4.  Interceptor 2:    Catches 401 → POST /api/auth/refresh (httpOnly cookie auto-sent)
5.  Server:           Returns new access token
6.  Interceptor 2:    Updates memory → retries GET /api/orders
7.  Component:        Receives data — never knew steps 3–6 happened
```

**Interview Q: How do you prevent multiple simultaneous token refresh calls?**
**A:** The `isRefreshing` flag and `failedQueue` pattern. The first 401 sets `isRefreshing = true`. All other 401s queue up instead of triggering their own refresh. Once refresh completes, all queued requests retry with the new token — only one refresh call to the server.

**One-liner:** Request interceptor = auto-attach token. Response interceptor = catch 401, silent refresh, retry. `isRefreshing` + `failedQueue` = prevents duplicate refresh race conditions.

---

### 🔴 19.5 PrivateRoute — protecting pages

```tsx
// src/features/auth/ui/PrivateRoute.tsx
export function PrivateRoute({ allowedRoles }: { allowedRoles?: string[] }) {
  const { user, isLoggedIn, isLoading } = useAuth()

  if (isLoading) return <div>Loading...</div>           // wait — don't redirect yet
  if (!isLoggedIn) return <Navigate to="/login" replace />
  if (allowedRoles && !allowedRoles.includes(user!.role))
    return <Navigate to="/403" replace />

  return <Outlet />
}

// Router setup
export const router = createBrowserRouter([
  { path: '/login', element: <LoginPage /> },
  { path: '/403',   element: <ForbiddenPage /> },

  {
    element: <PrivateRoute />,
    children: [
      { path: '/dashboard', element: <DashboardPage /> },
      { path: '/orders',    element: <OrdersPage /> },
    ]
  },
  {
    element: <PrivateRoute allowedRoles={['admin']} />,
    children: [
      { path: '/admin', element: <AdminPage /> },
    ]
  },
  {
    element: <PrivateRoute allowedRoles={['admin', 'manager']} />,
    children: [
      { path: '/reports', element: <ReportsPage /> },
    ]
  },
])
```

**Interview Q: How do you protect routes in React?**
**A:** PrivateRoute checks `isLoading` first (prevents flash), then `isLoggedIn` (redirects to /login), then role (redirects to /403). Uses `replace` so the back button doesn't loop.

**One-liner:** PrivateRoute checks isLoading → isLoggedIn → role. `replace` prevents back-button redirect loop.

---

### 🔴 19.6 RBAC — role-based access control

```ts
// JWT payload carries role AND permissions
// role = broad (admin / manager / user)
// permissions = granular (read:orders / write:orders / delete:orders)
```

```tsx
// Can component — declarative UI permission checks
export function Can({ role, permission, children, fallback = null }: CanProps) {
  const { user } = useAuth()
  if (!user) return <>{fallback}</>
  if (role) {
    const roles = Array.isArray(role) ? role : [role]
    if (!roles.includes(user.role)) return <>{fallback}</>
  }
  if (permission && !user.permissions.includes(permission)) return <>{fallback}</>
  return <>{children}</>
}

// Usage
function OrdersPage() {
  return (
    <div>
      <Can role={['admin', 'manager']}>
        <button>Export CSV</button>
      </Can>
      <Can permission="delete:orders">
        <button>Delete Selected</button>
      </Can>
      <Can role="admin" fallback={<p>Contact admin to edit</p>}>
        <EditOrderForm />
      </Can>
    </div>
  )
}
```

**IMPORTANT:** Frontend RBAC = UX only. Backend validates JWT on every request. Never trust frontend checks alone.

**One-liner:** Route level: PrivateRoute allowedRoles → /403. UI level: Can component → hide elements. Backend ALWAYS validates — frontend is UX only.

---

### 🔴 19.7 Login and logout flow end to end

**Login flow:**
```
1. POST /api/auth/login { email, password }
2. Server validates → generates access token (JWT, 15min) + refresh token (7 days)
3. Server responds: body = { accessToken, user } + Set-Cookie: refreshToken (httpOnly)
4. Frontend: setAccessToken(data.accessToken) → memory
             setUser(data.user) → AuthContext
5. Navigate to /dashboard
```

**Logout flow:**
```
1. POST /api/auth/logout
2. Server: deletes refresh token from DB + clears httpOnly cookie
3. Frontend: clearAccessToken() + setUser(null) + queryClient.clear() + broadcastLogout()
4. Navigate to /login
```

**Why `queryClient.clear()` on logout:**
```
Without: User B logs in after User A — sees User A's cached data → privacy violation
With:    Cache wiped on logout → User B starts fresh
```

**One-liner:** Login → tokens to memory + cookie → AuthContext updated → redirect. Logout → clear memory + clear cookie server-side + wipe cache + redirect.

---

### 🔴 19.8 Handling page refresh — restoring auth state

```tsx
useEffect(() => {
  async function checkAuth() {
    try {
      const res = await fetch('/api/auth/me', { credentials: 'include' })
      if (res.ok) {
        const data = await res.json()
        setAccessToken(data.accessToken)  // restore to memory
        setUser(data.user)                // restore to Context
      }
    } catch {
      setUser(null)
    } finally {
      setIsLoading(false)  // always stop loading
    }
  }
  checkAuth()
}, [])
```

**Flow:**
```
Page refresh → memory cleared → AuthProvider fires GET /api/auth/me
Browser auto-sends httpOnly cookie → server validates
Cookie valid   → returns { user, accessToken } → restored to memory + Context
Cookie expired → 401 → setUser(null) → PrivateRoute redirects to /login
isLoading = true the whole time → user never sees login page flash
```

**One-liner:** Page refresh = memory cleared. On load: GET /api/auth/me → cookie sent auto → restore token + user. `isLoading` prevents login flash.

---

### 🟡 19.9 Multi-tab sync with BroadcastChannel

```ts
// src/shared/lib/tabSync.ts
const channel = new BroadcastChannel('auth_sync')

export function broadcastTokenRefresh(token: string) {
  channel.postMessage({ type: 'TOKEN_REFRESHED', token })
}
export function broadcastLogout() {
  channel.postMessage({ type: 'LOGGED_OUT' })
}
export function listenForTabSync() {
  channel.onmessage = (event) => {
    if (event.data.type === 'TOKEN_REFRESHED') setAccessToken(event.data.token)
    if (event.data.type === 'LOGGED_OUT') {
      clearAccessToken()
      window.location.href = '/login'
    }
  }
}
```

**Without BroadcastChannel:** 3 tabs = 3 simultaneous refresh calls = race condition on server.
**With BroadcastChannel:** Tab 1 refreshes → broadcasts to Tab 2 and Tab 3 → 1 call total.

**One-liner:** BroadcastChannel syncs token refresh across tabs. Refresh in one → all update. Logout in one → all redirect.

---

### 🟡 19.10 Auth providers — build vs buy

| | Custom JWT | Auth0 | Clerk | Supabase Auth |
|--|-----------|-------|-------|---------------|
| Setup time | Weeks | Hours | Minutes | Hours |
| Control | Full | Partial | Partial | Partial |
| Social login | Build yourself | Built in | Built in | Built in |
| MFA | Build yourself | Built in | Built in | Built in |
| Best for | Existing backend | Large apps, compliance | Startups | Supabase backend |

**One-liner:** Custom JWT = full control, weeks. Auth0 = enterprise features, hours. Clerk = fastest DX, minutes. Choose based on control needed vs time to ship.

---

## Top Interview Questions — Auth

**Q1: What is JWT?** JWT = 3-part base64 token. Header (algo) + Payload (user data) + Signature (proof). Server signs it. Frontend sends it. Not encrypted — just signed. Never put secrets in it.

**Q2: Why not localStorage?** Any JS on the page can read localStorage — XSS steals it with one line. httpOnly cookie = JS cannot read it. Memory = external scripts cannot access it.

**Q3: How does token attach to every call?** Axios request interceptor reads memory, adds `Authorization: Bearer` automatically. Response interceptor catches 401, silently refreshes, retries. Component never knows.

**Q4: What happens on page refresh?** Memory cleared. AuthContext fires `GET /api/auth/me`. Browser sends httpOnly cookie auto. Server returns new token + user. Restored silently. `isLoading` prevents flash.

**Q5: Multiple tabs?** BroadcastChannel. One tab refreshes → broadcasts new token → all tabs update. Logout → all tabs redirect. Prevents duplicate refresh race conditions.

**Q6: RBAC?** JWT carries role + permissions[]. Route level: PrivateRoute redirects to /403. UI level: Can component hides elements. Backend validates everything — frontend is UX only.

**Q7: Logout?** POST /api/auth/logout → server deletes refresh token + clears cookie. Frontend clears memory + resets Context + `queryClient.clear()` + BroadcastChannel logout.

**Q8: Access vs refresh token?** Access: 15min, has user data, in memory, sent with every call. Refresh: 7 days, reference ID only, httpOnly cookie, only used to get new access token.

---

## Quick Revision — Category 19 One-Liners

```
JWT = header.payload.signature. Base64, NOT encrypted. Never put secrets in it.
Access token = 15min, memory. Refresh token = 7 days, httpOnly cookie.
localStorage = any JS steals it. Memory = safe. httpOnly cookie = browser only.
Cookie flags: HttpOnly + Secure + SameSite=Strict

AuthContext: user, isLoggedIn, isLoading, login(), logout()
isLoading prevents login flash on app startup
queryClient.clear() on logout = next user won't see old cached data

Request interceptor = auto-attach token. Response interceptor = 401 → silent refresh → retry
isRefreshing + failedQueue = one refresh call even if 5 requests fail simultaneously

PrivateRoute: isLoading → isLoggedIn → role. replace prevents back-button loop
RBAC: PrivateRoute for routes, Can component for UI. Backend ALWAYS validates — frontend is UX.

BroadcastChannel: TOKEN_REFRESHED → all tabs sync. LOGGED_OUT → all tabs redirect.
Custom JWT = control. Auth0 = features fast. Clerk = fastest DX.
```

---

# Category 20: WebSocket & Real-Time Communication

---

## The 5 Points That Show Senior-Level Thinking — WebSocket

> When an interviewer asks **"How did you implement real-time features in your app?"**
> these are the 5 points that separate a senior answer from a junior answer.
> A junior says "we used setInterval to poll the API every few seconds."

---

### Point 1 · We understood when WebSocket is actually needed — not everything needs it

**What juniors say:**
> "We used WebSocket for all our data fetching."

**What seniors say:**
> "We first asked whether the feature truly needs real-time server push. If data
> can be slightly stale — like a dashboard refreshing every 30 seconds — we used
> TanStack Query's refetchInterval. WebSocket adds persistent connection complexity,
> so we only used it where data needed to be instant — live order status and
> in-app notifications."

**Why this impresses:** You show judgement. Reaching for WebSocket for everything is a red flag.

---

### Point 2 · We used Socket.IO over native WebSocket for auto-reconnect and fallback

**What juniors say:**
> "We used the browser's built-in WebSocket API."

**What seniors say:**
> "We used Socket.IO because it adds automatic reconnection, named events instead
> of raw message strings, room support for targeting specific users, and a fallback
> to HTTP long-polling if WebSocket is blocked by a corporate firewall — which
> happens often in enterprise environments."

**Why this impresses:** Mentioning corporate firewalls shows real production experience.

---

### Point 3 · We created a custom hook to manage the WebSocket lifecycle cleanly

**What juniors say:**
> "We set up the WebSocket connection directly inside the component."

**What seniors say:**
> "We extracted all WebSocket logic into a custom hook. The hook manages connecting
> on mount, handling incoming events, cleaning up on unmount, and exposing a simple
> API. Components never deal with the socket directly."

**Why this impresses:** Applying custom hooks to non-trivial lifecycle problems shows architecture thinking.

---

### Point 4 · We combined WebSocket with TanStack Query — WS triggers cache invalidation

**What juniors say:**
> "We stored the WebSocket data in a separate useState, different from the REST data."

**What seniors say:**
> "Hybrid pattern. TanStack Query fetches initial data via REST. Then a Socket.IO
> connection listens for server events. When the server pushes an update, the
> handler calls queryClient.invalidateQueries — TanStack Query refetches
> automatically. One source of truth — the TanStack Query cache."

**Why this impresses:** Two separate stores causes sync bugs. Invalidating the cache is the production-grade pattern.

---

### Point 5 · We always cleaned up the WebSocket connection to prevent memory leaks

**What juniors say:**
> "We opened the WebSocket connection in useEffect."

**What seniors say:**
> "We always returned a cleanup function that disconnects the socket. Without
> cleanup, navigating away leaves the connection open — server keeps sending,
> handlers keep running. If the component remounts a second connection opens.
> In production this caused a ghost listener bug where users received duplicate
> notifications."

**Why this impresses:** Mentioning a specific production bug shows real-world debugging experience.

---

### 🔴 20.1 REST pull vs WebSocket push — when to use which

**Explanation:** These are two different communication models — choose based on whether the server needs to push data without being asked.

```
REST (pull):    Browser asks → Server responds → Connection closes
WebSocket (push): Persistent connection — server pushes any time
```

**Decision table:**

| Feature | Best tool | Reason |
|---------|-----------|--------|
| Live chat | WebSocket | Instant, two-way |
| Stock prices | WebSocket | Server-initiated, instant |
| Live order tracking | WebSocket | Server pushes on state change |
| In-app notifications | WebSocket / SSE | Server initiates |
| Analytics dashboard | `refetchInterval` | 30s stale is fine |
| Search results | REST | User triggers |
| User profile | REST + cache | Changes rarely |

**Interview Q: How do you decide between REST and WebSocket?**
**A:** First check if data truly needs to be pushed by the server without the user doing anything. If yes, WebSocket. If data can be seconds or minutes stale, TanStack Query's `refetchInterval` is simpler. WebSocket only for genuinely instant two-way or server-push communication.

**Common Mistake:** Using WebSocket for features that don't need it. `refetchInterval: 30000` in TanStack Query is far simpler and has zero connection complexity.

**One-liner:** WebSocket = server pushes instantly. REST + polling = data can be slightly stale. Don't reach for WebSocket unless you need true real-time push.

---

### 🔴 20.2 Native WebSocket API in React

**Explanation:** The browser has a built-in WebSocket API. Open in `useEffect` — MUST close in the cleanup function.

```tsx
function LivePrices() {
  const [price, setPrice] = useState<number | null>(null)
  const [status, setStatus] = useState<'connecting' | 'connected' | 'disconnected'>('connecting')

  useEffect(() => {
    const ws = new WebSocket('wss://api.example.com/prices')

    ws.onopen = () => {
      setStatus('connected')
      ws.send(JSON.stringify({ subscribe: 'BTC-USD' }))
    }
    ws.onmessage = (event) => {
      const data = JSON.parse(event.data)
      setPrice(data.price)
    }
    ws.onerror = () => setStatus('disconnected')
    ws.onclose = () => setStatus('disconnected')

    // CLEANUP — critical. Without this:
    // socket stays open after unmount → memory leak → ghost handlers
    // on remount: second socket opens → duplicate events
    return () => ws.close()
  }, [])

  return (
    <div>
      <p>Status: {status}</p>
      <p>Price: {price ? `$${price}` : 'Loading...'}</p>
    </div>
  )
}
```

**Interview Q: Why must you close a WebSocket in useEffect cleanup?**
**A:** Without cleanup the socket stays open after unmount. Server keeps sending, handlers keep running on a gone component. If user navigates back, a second socket opens — two sockets fire events — user sees duplicate data and memory leaks grow.

**Common Mistake:** Omitting `[]` dependency — a new connection opens on every render.

**One-liner:** Open in useEffect. ALWAYS close in cleanup return. No cleanup = memory leak + ghost handlers + duplicate events.

---

### 🔴 20.3 Socket.IO — why it is preferred in enterprise

**Explanation:** Socket.IO adds what native WebSocket lacks — reconnection, named events, rooms, and HTTP fallback.

```
Native WebSocket problems:        Socket.IO solutions:
  No auto-reconnect            →  Retries automatically on disconnect
  Firewalls may block WS       →  Falls back to HTTP long-polling
  Raw string messages          →  Named events: socket.on('orderUpdated', handler)
  No rooms / targeting         →  socket.join('order:123') to target specific users
```

```tsx
// src/shared/lib/socket.ts
import { io, Socket } from 'socket.io-client'

let socket: Socket | null = null

export function getSocket(): Socket {
  if (!socket) {
    socket = io(import.meta.env.VITE_WS_URL, {
      auth: { token: getAccessToken() },
      transports: ['websocket', 'polling'],  // WS first, fallback to polling
      reconnection: true,
      reconnectionAttempts: 5,
      reconnectionDelay: 1000,
    })
  }
  return socket
}

// Using in a component
function OrderTracker({ orderId }: { orderId: string }) {
  const [status, setStatus] = useState('pending')

  useEffect(() => {
    const socket = getSocket()
    socket.emit('joinRoom', `order:${orderId}`)

    socket.on('orderStatusChanged', (data) => {
      if (data.orderId === orderId) setStatus(data.status)
    })

    return () => {
      socket.emit('leaveRoom', `order:${orderId}`)
      socket.off('orderStatusChanged')  // MUST remove or listeners stack up
    }
  }, [orderId])

  return <p>Status: {status}</p>
}
```

**Interview Q: Why use Socket.IO over native WebSocket?**
**A:** Three production reasons — automatic reconnection (network flicker = auto-reconnect), HTTP fallback (corporate firewalls often block WebSocket — Socket.IO silently falls back), and named events + rooms (clean `socket.on('orderUpdated', handler)` instead of parsing raw JSON strings).

**Common Mistake:** Not calling `socket.off()` in cleanup. Without it, every remount stacks up more listeners — handler fires multiple times per event.

**One-liner:** Socket.IO = WebSocket + auto-reconnect + HTTP fallback + named events + rooms. Always `socket.off()` in cleanup.

---

### 🔴 20.4 Custom useSocket hook

**Explanation:** Extract all WebSocket lifecycle into custom hooks — components only care about the data.

```tsx
// Reusable event listener hook
export function useSocketEvent<T>(event: string, handler: (data: T) => void) {
  useEffect(() => {
    const socket = getSocket()
    socket.on(event, handler)
    return () => { socket.off(event, handler) }
  }, [event, handler])
}

// Reusable room hook
export function useSocketRoom(roomId: string) {
  useEffect(() => {
    if (!roomId) return
    const socket = getSocket()
    socket.emit('joinRoom', roomId)
    return () => { socket.emit('leaveRoom', roomId) }
  }, [roomId])
}

// Full feature hook — all socket logic in one place
export function useOrderSocket(orderId: string) {
  const [status, setStatus] = useState('pending')
  const [eta, setEta] = useState<string | null>(null)

  useEffect(() => {
    const socket = getSocket()
    socket.emit('joinRoom', `order:${orderId}`)

    socket.on('orderStatusChanged', (d) => { if (d.orderId === orderId) setStatus(d.status) })
    socket.on('etaUpdated', (d) => { if (d.orderId === orderId) setEta(d.eta) })

    return () => {
      socket.emit('leaveRoom', `order:${orderId}`)
      socket.off('orderStatusChanged')
      socket.off('etaUpdated')
    }
  }, [orderId])

  return { status, eta }
}

// Component is completely clean
function OrderPage({ orderId }: { orderId: string }) {
  const { status, eta } = useOrderSocket(orderId)
  return <div><p>Status: {status}</p>{eta && <p>ETA: {eta}</p>}</div>
}
```

**One-liner:** Custom hook = connect on mount, handle events, clean up on unmount, expose only data. Components stay clean — zero socket logic inside them.

---

### 🔴 20.5 TanStack Query + WebSocket — the hybrid pattern

**Explanation:** REST for initial load + cache. WebSocket triggers cache invalidation. One source of truth.

```tsx
function OrdersPage() {
  // TanStack Query: initial load + caching
  const { data: orders } = useQuery({
    queryKey: ['orders'],
    queryFn: fetchOrders,
  })

  // WebSocket: listen for updates → invalidate cache
  useEffect(() => {
    const socket = getSocket()

    // New order created → refetch list
    socket.on('orderCreated', () => {
      queryClient.invalidateQueries({ queryKey: ['orders'] })
    })

    // Specific order status changed → update that item in cache directly
    socket.on('orderStatusChanged', (data) => {
      queryClient.setQueryData(['orders'], (old: Order[] | undefined) =>
        old?.map(o => o.id === data.orderId ? { ...o, status: data.status } : o)
      )
    })

    return () => {
      socket.off('orderCreated')
      socket.off('orderStatusChanged')
    }
  }, [])

  return <OrderList orders={orders} />
}
```

**Why NOT two separate stores:**
```
❌ Separate stores — sync issues
  const [orders, setOrders] = useState([])      // REST data
  const [liveOrders, setLiveOrders] = useState([]) // WS data — gets out of sync

✅ One source of truth — TanStack Query cache
  REST loads initial data. WS invalidates or updates cache.
  Component reads from one place. Always consistent.
```

**Interview Q: How did you combine WebSocket with TanStack Query?**
**A:** Hybrid pattern. TanStack Query handles initial REST fetch — cached, instant on revisit. Socket.IO listens for server events. When an event fires, the handler calls `queryClient.invalidateQueries` — TanStack Query refetches automatically. One source of truth. REST for initial load, WebSocket for live updates.

**One-liner:** TanStack Query = initial load + cache. WS event → `invalidateQueries` → auto refetch. One source of truth. Never two separate stores for the same data.

---

### 🟡 20.6 Zustand + WebSocket — storing live UI state

**Explanation:** Use Zustand for WebSocket data that is purely UI state — notifications, typing indicators, presence. These have no REST equivalent.

```tsx
// Zustand store for notifications
const useNotificationStore = create<NotificationStore>((set) => ({
  notifications: [],
  unreadCount: 0,
  addNotification: (n) => set((s) => ({
    notifications: [n, ...s.notifications].slice(0, 50),
    unreadCount: s.unreadCount + 1,
  })),
  markAllRead: () => set({ unreadCount: 0 }),
}))

// Connect WebSocket to Zustand — call once after login
export function connectNotificationSocket() {
  const socket = getSocket()
  socket.on('notification', (data) => {
    useNotificationStore.getState().addNotification(data)
  })
}

// Any component reads from Zustand
function NotificationBell() {
  const unreadCount = useNotificationStore(s => s.unreadCount)
  return <span>{unreadCount > 0 && <span>{unreadCount}</span>}</span>
}
```

**Rule:**
```
WebSocket data that IS in your REST API (orders, products):
  → TanStack Query (invalidateQueries / setQueryData)

WebSocket data that is UI-only (notifications, typing, who is online):
  → Zustand (no REST equivalent for this data)
```

**One-liner:** Zustand for WS UI-only data (notifications, typing, presence). TanStack Query for WS updates to API data. Never mix — one store per data type.

---

### 🟡 20.7 Polling vs WebSocket vs SSE — comparison

| | Polling (`refetchInterval`) | SSE | WebSocket |
|--|--------------------------|-----|-----------|
| Direction | Client → Server only | Server → Client only | Both ways |
| Connection | New HTTP request each time | One persistent HTTP | One persistent TCP |
| Complexity | Very simple | Simple | More complex |
| Real-time | No (interval delay) | Yes | Yes |
| Use for | Dashboard metrics | Notifications, live feed | Chat, collaborative |

```tsx
// SSE — simpler than WebSocket for one-way server push
function LiveFeed() {
  const [events, setEvents] = useState<string[]>([])

  useEffect(() => {
    const eventSource = new EventSource('/api/events', { withCredentials: true })
    eventSource.onmessage = (e) => setEvents(prev => [e.data, ...prev].slice(0, 20))
    eventSource.onerror = () => eventSource.close()
    return () => eventSource.close()  // always close on unmount
  }, [])

  return <ul>{events.map((e, i) => <li key={i}>{e}</li>)}</ul>
}
```

**Interview Q: When would you use SSE instead of WebSocket?**
**A:** SSE when you only need server → client push — notifications, activity feeds, live logs. SSE uses regular HTTP (works through proxies/firewalls easily) and auto-reconnects natively. WebSocket when you need bidirectional — chat, collaborative editing, multiplayer, trading.

**One-liner:** Polling = simple, not real-time. SSE = server-to-client only, simple HTTP. WebSocket = bidirectional, real-time. Use the simplest tool that meets the requirement.

---

## Top Interview Questions — WebSocket

**Q1: REST vs WebSocket?** WebSocket when server must push instantly without being asked (chat, live prices). REST + polling when data can be slightly stale (dashboards). WebSocket adds connection complexity — only use when truly needed.

**Q2: Memory leaks?** Always return cleanup from `useEffect` that closes socket and removes listeners. No cleanup = open socket after unmount + ghost handlers + duplicate events on remount.

**Q3: Socket.IO authentication?** Pass token in `auth` option: `io(url, { auth: { token: getAccessToken() } })`. Server validates in Socket.IO middleware before establishing connection.

**Q4: TanStack Query + WebSocket?** Hybrid pattern — TanStack Query for initial REST load and cache. Socket.IO event fires → `queryClient.invalidateQueries` → auto refetch. One source of truth. No duplicate state.

**Q5: SSE vs WebSocket?** SSE = server-to-client only, regular HTTP, simpler. WebSocket = bidirectional. Use SSE for notifications/feeds. Use WebSocket for chat/collaborative/bidirectional.

**Q6: Reconnection?** Native WebSocket requires manual reconnect logic. Socket.IO handles it automatically with `reconnectionAttempts` and `reconnectionDelay`. On reconnect, re-emit `joinRoom` for any rooms the user was in.

---

## Quick Revision — Category 20 One-Liners

```
FUNDAMENTALS
  WebSocket = persistent two-way. Server pushes without being asked.
  REST = client pulls. WebSocket = server pushes. Not everything needs WebSocket.
  refetchInterval in TanStack Query covers many "real-time" dashboard needs.

WHEN TO USE WHAT
  WebSocket: chat, live prices, live order tracking, collaborative features
  SSE: notifications, activity feed, log streaming (server-to-client only)
  Polling: dashboards, leaderboards (slightly stale is OK, much simpler)
  REST: search, user-triggered requests

NATIVE WEBSOCKET
  Open in useEffect. ALWAYS close in cleanup return.
  No cleanup = memory leak + ghost handlers + duplicate events.
  [] dependency = connect once on mount, disconnect on unmount.

SOCKET.IO
  Adds: auto-reconnect + named events + rooms + HTTP fallback
  Corporate firewalls block WebSocket → fallback keeps app working
  socket.on() always paired with socket.off() in cleanup

CUSTOM HOOKS
  useSocketEvent(event, handler) + useSocketRoom(roomId) = reusable
  Components get clean data — zero socket logic inside them

TANSTACK QUERY + WEBSOCKET
  TanStack Query = initial REST load + cache
  WS event fires → invalidateQueries → auto refetch
  One source of truth. Never two separate stores for same data.

ZUSTAND + WEBSOCKET
  For UI-only WS data: notifications, typing, presence
  TanStack Query for WS updates to API data. Zustand for WS-only UI data.

POLLING vs SSE vs WEBSOCKET
  Polling = simple, not real-time
  SSE = server-to-client only, simple HTTP, auto-reconnects
  WebSocket = bidirectional, real-time, Socket.IO for production
```

[↑ Back to Table of Contents](#table-of-contents)