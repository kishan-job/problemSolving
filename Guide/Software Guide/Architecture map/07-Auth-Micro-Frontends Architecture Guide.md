# Micro-Frontends Architecture Guide

> A complete guide to building, deploying, and managing independent React applications that work together seamlessly while maintaining separate authentication and state management.

---

# PART 1 — What are Micro-Frontends?

## Definition

**Micro-frontends** = Breaking a frontend monolith into smaller, **independently deployable** React/Vue/Angular applications that together form one cohesive user interface.

Think of it like a TV channel that shows multiple programs:

```
User Browser
    ↓
Main Shell App (the "TV remote")
├── Controls navigation
├── Manages global state
└── Loads micro-apps dynamically
    ↓
Micro-App 1 (Video Player)        ← Separate webpack build
Micro-App 2 (Checkout)            ← Separate webpack build
Micro-App 3 (Admin Panel)         ← Separate webpack build
Micro-App 4 (Billing)             ← Separate webpack build
```

Each app:

- ✅ Built independently
- ✅ Deployed independently
- ✅ Can be developed by different teams
- ✅ Shares authentication with other apps
- ✅ Can communicate with sibling apps

---

## Micro-Frontends vs Monorepo (Turborepo)

| Aspect                | Turborepo                                       | Micro-Frontends                            |
| --------------------- | ----------------------------------------------- | ------------------------------------------ |
| **Build**             | One build for all apps                          | Each app built separately                  |
| **Deployment**        | All apps deploy together                        | Each app deploys independently             |
| **Shared Code**       | Via workspace packages                          | Via shared dependencies or runtime loading |
| **Dev Speed**         | Fast (caching, incremental builds)              | Fast (each dev works on one app)           |
| **Team Independence** | Medium (shared configs)                         | High (completely separate)                 |
| **Complexity**        | Low-Medium                                      | Medium-High                                |
| **Real Example**      | DirecTV monorepo (apps/overview, apps/checkout) | Netflix, Shopify (hundreds of apps)        |

---

# PART 2 — How Apps Are Built

## Traditional Monorepo Build (Turborepo)

```
Monorepo Root
├── apps/web
├── apps/checkout
└── packages/shared
    ↓
Single Build Process
├── Transpile TypeScript → JavaScript
├── Bundle with webpack/Turbopack
├── Code split
├── Output: static files
    ↓
Build Output
├── web/.next/
├── checkout/.next/
└── All built together
    ↓
Deploy all at once
```

**Turborepo Build Command:**

```bash
turbo run build
# Runs build in all apps sequentially/parallel with caching
```

**Result:** One unified monorepo deployment (all apps built together).

---

## Micro-Frontends Build (Multiple Independent Builds)

```
Micro-Frontends Architecture
├── Shell App (Host)
│   ├── webpack.config.js
│   ├── Contains: Shell UI, routing
│   └── Can load remote apps dynamically
│
├── Remote App 1 (Video)
│   ├── webpack.config.js (SEPARATE)
│   ├── Contains: Video player logic
│   └── Exposes module federation
│
├── Remote App 2 (Checkout)
│   ├── webpack.config.js (SEPARATE)
│   ├── Contains: Checkout flows
│   └── Exposes module federation
│
└── Remote App 3 (Billing)
    ├── webpack.config.js (SEPARATE)
    ├── Contains: Billing UI
    └── Exposes module federation
    ↓
Independent Build Process 1 → video.js
Independent Build Process 2 → checkout.js
Independent Build Process 3 → billing.js
    ↓
Each app deployed to CDN
    ↓
Shell loads apps at runtime (dynamically)
```

**Micro-Frontend Build Commands (each app separate):**

```bash
# Shell builds independently
cd shell-app && npm run build

# Video app builds independently
cd apps/video && npm run build

# Checkout app builds independently
cd apps/checkout && npm run build

# Billing app builds independently
cd apps/billing && npm run build
```

**Result:** 4 completely separate webpack builds, each deployed to different URLs.

---

# PART 3 — Module Federation (The Magic)

## What is Module Federation?

**Module Federation** = A webpack feature that lets apps:

1. Share code at **runtime** (not build time)
2. Load remote modules dynamically
3. Export their own modules for others to use

Think of it like software plugins:

```
Shell App (Host) — the main application
    ↓
Plugin System (Module Federation)
├── Plugin 1: Video Player
├── Plugin 2: Checkout
└── Plugin 3: Billing

Each plugin is:
- Loaded at runtime
- Can be updated without restarting host
- Shares dependencies with host
```

---

## Module Federation Configuration (webpack)

### Shell App (`shell/webpack.config.js`)

```javascript
const ModuleFederationPlugin =
  require('webpack').container.ModuleFederationPlugin;
const deps = require('./package.json').dependencies;

module.exports = {
  mode: 'production',
  entry: './src/index',
  output: {
    path: __dirname + '/dist',
    filename: 'shell.js'
  },
  devServer: {
    port: 3000
  },
  resolve: {
    extensions: ['.js', '.jsx', '.ts', '.tsx']
  },
  module: {
    rules: [
      {
        test: /\.tsx?$/,
        use: 'babel-loader',
        exclude: /node_modules/
      }
    ]
  },
  plugins: [
    new ModuleFederationPlugin({
      name: 'shell', // ← App name
      filename: 'remoteEntry.js', // ← Entry point for others
      remotes: {
        // Shell consumes these remote apps
        video: 'video@http://localhost:3001/remoteEntry.js',
        checkout: 'checkout@http://localhost:3002/remoteEntry.js',
        billing: 'billing@http://localhost:3003/remoteEntry.js'
      },
      shared: {
        // Share these dependencies across all apps
        react: { singleton: true, requiredVersion: deps.react },
        'react-dom': { singleton: true, requiredVersion: deps['react-dom'] },
        'react-router-dom': {
          singleton: true,
          requiredVersion: deps['react-router-dom']
        }
      }
    })
  ]
};
```

### Video App (`apps/video/webpack.config.js`)

```javascript
const ModuleFederationPlugin =
  require('webpack').container.ModuleFederationPlugin;
const deps = require('./package.json').dependencies;

module.exports = {
  mode: 'production',
  entry: './src/index',
  output: {
    path: __dirname + '/dist',
    filename: 'video.js',
    publicPath: 'http://localhost:3001/' // ← Where this app is deployed
  },
  devServer: {
    port: 3001
  },
  resolve: {
    extensions: ['.js', '.jsx', '.ts', '.tsx']
  },
  module: {
    rules: [
      {
        test: /\.tsx?$/,
        use: 'babel-loader',
        exclude: /node_modules/
      }
    ]
  },
  plugins: [
    new ModuleFederationPlugin({
      name: 'video', // ← App name
      filename: 'remoteEntry.js', // ← Entry point
      exposes: {
        // Video app exposes these modules
        './VideoPlayer': './src/VideoPlayer',
        './VideoContext': './src/context/VideoContext'
      },
      shared: {
        react: { singleton: true, requiredVersion: deps.react },
        'react-dom': { singleton: true, requiredVersion: deps['react-dom'] }
      }
    })
  ]
};
```

### Checkout App (`apps/checkout/webpack.config.js`)

```javascript
const ModuleFederationPlugin =
  require('webpack').container.ModuleFederationPlugin;
const deps = require('./package.json').dependencies;

module.exports = {
  mode: 'production',
  entry: './src/index',
  output: {
    path: __dirname + '/dist',
    filename: 'checkout.js',
    publicPath: 'http://localhost:3002/'
  },
  devServer: {
    port: 3002
  },
  resolve: {
    extensions: ['.js', '.jsx', '.ts', '.tsx']
  },
  module: {
    rules: [
      {
        test: /\.tsx?$/,
        use: 'babel-loader',
        exclude: /node_modules/
      }
    ]
  },
  plugins: [
    new ModuleFederationPlugin({
      name: 'checkout',
      filename: 'remoteEntry.js',
      exposes: {
        './Checkout': './src/Checkout',
        './Cart': './src/components/Cart',
        './Payment': './src/components/Payment'
      },
      shared: {
        react: { singleton: true, requiredVersion: deps.react },
        'react-dom': { singleton: true, requiredVersion: deps['react-dom'] }
      }
    })
  ]
};
```

---

## How Module Federation Works (Runtime Loading)

```
Timeline:

1. User visits Shell App (http://localhost:3000)
   ↓
2. Shell's index.html loads shell.js
   ↓
3. Shell renders shell/remoteEntry.js
   This file contains: { video: {...}, checkout: {...}, billing: {...} }
   ↓
4. User clicks "Watch Video"
   ↓
5. Shell dynamically imports from Video app:
   import('video/VideoPlayer')
   ↓
6. Browser requests http://localhost:3001/remoteEntry.js
   ↓
7. Browser receives and caches Video app's remoteEntry
   ↓
8. Browser loads video.js from CDN
   ↓
9. Video app mounts in Shell's container
   ↓
10. Video app can share React context with Shell
   ↓
11. User sees video player (no page refresh)
```

---

## Dynamic Import (Loading Remote Apps)

### Shell App Loading Video

```typescript
// shell/src/pages/VideoPage.tsx
import React, { Suspense, lazy } from 'react';

// Dynamically import Video app at runtime
const VideoPlayer = lazy(() => import('video/VideoPlayer'));

export default function VideoPage() {
  return (
    <Suspense fallback={<div>Loading video player...</div>}>
      <VideoPlayer />
    </Suspense>
  );
}
```

**What happens:**

1. When component mounts → requests video app
2. Browser fetches `http://localhost:3001/remoteEntry.js`
3. Caches it (won't fetch again)
4. Loads `VideoPlayer` from video app
5. Renders in place

### Shell App Loading Checkout

```typescript
// shell/src/pages/CheckoutPage.tsx
import React, { Suspense, lazy } from 'react';

const Checkout = lazy(() => import('checkout/Checkout'));

export default function CheckoutPage() {
  return (
    <Suspense fallback={<div>Loading checkout...</div>}>
      <Checkout />
    </Suspense>
  );
}
```

---

# PART 4 — Shared Dependencies

## Problem: Duplicate Libraries

Without sharing:

```
Shell App loads:
├── react 18.0.0 (100kb)
├── react-dom 18.0.0 (200kb)
└── react-router-dom 6.0.0 (50kb)

Video App loads:
├── react 18.0.0 (100kb)   ← DUPLICATE
├── react-dom 18.0.0 (200kb) ← DUPLICATE
└── axios 1.0.0 (20kb)

Checkout App loads:
├── react 18.0.0 (100kb)   ← DUPLICATE
└── react-dom 18.0.0 (200kb) ← DUPLICATE

Total Download: ~920kb
```

## Solution: Singleton Pattern

With Module Federation sharing:

```javascript
// All apps config has:
shared: {
  react: {
    singleton: true,        // ← Only load ONCE
    requiredVersion: '^18.0.0'
  },
  'react-dom': {
    singleton: true,        // ← Only load ONCE
    requiredVersion: '^18.0.0'
  },
}

Result:
├── Shell loads react (100kb) + react-dom (200kb)
├── Video uses Shell's react + react-dom (0kb extra)
└── Checkout uses Shell's react + react-dom (0kb extra)

Total Download: ~320kb (65% reduction!)
```

---

## Shared Dependencies Configuration

### Centralized Shared Dependencies

```javascript
// shared-deps.js (reused in all apps)
const deps = require('./package.json').dependencies;

module.exports = {
  react: {
    singleton: true,
    requiredVersion: deps.react
  },
  'react-dom': {
    singleton: true,
    requiredVersion: deps['react-dom']
  },
  'react-router-dom': {
    singleton: true,
    requiredVersion: deps['react-router-dom']
  },
  axios: {
    singleton: true,
    requiredVersion: deps.axios
  },
  zustand: {
    singleton: true,
    requiredVersion: deps.zustand
  }
};
```

Then in each app:

```javascript
const { ModuleFederationPlugin } = require('webpack').container;
const sharedDeps = require('../../shared-deps');

module.exports = {
  plugins: [
    new ModuleFederationPlugin({
      name: 'video',
      filename: 'remoteEntry.js',
      exposes: { './VideoPlayer': './src/VideoPlayer' },
      shared: sharedDeps // ← Share centrally defined deps
    })
  ]
};
```

---

# PART 5 — Build Process Comparison

## Turborepo Build Process

```
Step 1: Start at root
$ turbo run build

Step 2: Turbo analyzes dependencies
├── apps/web depends on @company/auth-client
├── apps/checkout depends on @company/auth-client
└── packages/auth-client has no deps

Step 3: Build order (with caching)
├── Build packages/auth-client
├── Build apps/web (waits for packages/auth-client)
└── Build apps/checkout (waits for packages/auth-client)

Step 4: Output
├── apps/web/.next/
├── apps/checkout/.next/
├── packages/auth-client/dist/
└── All built into single monorepo

Step 5: Deploy as unit
All apps deployed together to CDN/hosting
```

**Build Command:**

```bash
turbo run build
# Output: apps/.next/, packages/dist/
# Time: ~5-10 minutes (all apps)
# Deploy: One command deploys all
```

---

## Micro-Frontends Build Process

```
Step 1: Build Shell independently
$ cd shell && npm run build
├── Compile TypeScript
├── Bundle with webpack
├── Output: dist/shell.js, dist/remoteEntry.js
├── Upload to CDN: https://cdn.example.com/shell/

Step 2: Build Video App independently
$ cd apps/video && npm run build
├── Compile TypeScript
├── Bundle with webpack
├── Output: dist/video.js, dist/remoteEntry.js
├── Upload to CDN: https://cdn.example.com/video/

Step 3: Build Checkout App independently
$ cd apps/checkout && npm run build
├── Compile TypeScript
├── Bundle with webpack
├── Output: dist/checkout.js, dist/remoteEntry.js
├── Upload to CDN: https://cdn.example.com/checkout/

Step 4: Build Billing App independently
$ cd apps/billing && npm run build
├── Compile TypeScript
├── Bundle with webpack
├── Output: dist/billing.js, dist/remoteEntry.js
├── Upload to CDN: https://cdn.example.com/billing/

Step 5: Deploy independently
Video team deploys video app without waiting
Checkout team deploys checkout app independently
No redeploy needed for other teams
```

**Build Commands (each separate):**

```bash
# Can run in parallel
cd shell && npm run build &
cd apps/video && npm run build &
cd apps/checkout && npm run build &
cd apps/billing && npm run build &

# Wait for all to finish
wait

# Each deploys independently to CDN
```

**Time:** ~1-2 minutes per app (can run in parallel = ~1-2 minutes total)  
**Deploy:** Each app deployed separately, no coordination needed

---

# PART 6 — Complete Micro-Frontends Example

## Directory Structure

```
micro-frontends-monorepo/
├── shell-app/                    ← Host/Container app
│   ├── webpack.config.js
│   ├── package.json
│   ├── src/
│   │   ├── index.tsx
│   │   ├── App.tsx              (Routing, Navigation)
│   │   ├── Layout.tsx           (Shell layout)
│   │   ├── pages/
│   │   │   ├── VideoPage.tsx
│   │   │   ├── CheckoutPage.tsx
│   │   │   └── BillingPage.tsx
│   │   └── context/
│   │       └── AuthContext.tsx  (Shared auth)
│   └── dist/                    (Build output)
│
├── apps/
│   ├── video/                   ← Remote App 1
│   │   ├── webpack.config.js
│   │   ├── package.json
│   │   ├── src/
│   │   │   ├── index.tsx
│   │   │   ├── VideoPlayer.tsx
│   │   │   └── context/
│   │   │       └── VideoContext.tsx
│   │   └── dist/
│   │
│   ├── checkout/                ← Remote App 2
│   │   ├── webpack.config.js
│   │   ├── package.json
│   │   ├── src/
│   │   │   ├── index.tsx
│   │   │   ├── Checkout.tsx
│   │   │   └── components/
│   │   │       ├── Cart.tsx
│   │   │       └── Payment.tsx
│   │   └── dist/
│   │
│   └── billing/                 ← Remote App 3
│       ├── webpack.config.js
│       ├── package.json
│       ├── src/
│       │   ├── index.tsx
│       │   ├── Billing.tsx
│       │   └── components/
│       │       └── Invoice.tsx
│       └── dist/
│
├── packages/
│   ├── shared-auth/             ← Shared auth logic
│   │   ├── src/
│   │   │   ├── useAuth.ts
│   │   │   ├── AuthProvider.tsx
│   │   │   └── getSession.ts
│   │   └── package.json
│   │
│   └── shared-utils/            ← Shared utilities
│       ├── src/
│       │   ├── api.ts
│       │   └── logger.ts
│       └── package.json
│
├── .github/workflows/
│   ├── shell.yml                ← Deploy shell
│   ├── video.yml                ← Deploy video
│   ├── checkout.yml             ← Deploy checkout
│   └── billing.yml              ← Deploy billing
│
└── package.json (root)
```

---

## Shell App (`shell-app/src/App.tsx`)

```typescript
import React, { Suspense } from 'react';
import { BrowserRouter, Routes, Route } from 'react-router-dom';
import { AuthProvider } from 'shared-auth';
import Layout from './Layout';

// Lazy load remote apps
const VideoPage = React.lazy(() => import('video/VideoPlayer').then(m => ({ default: m.VideoPage })));
const CheckoutPage = React.lazy(() => import('checkout/Checkout').then(m => ({ default: m.CheckoutPage })));
const BillingPage = React.lazy(() => import('billing/Billing').then(m => ({ default: m.BillingPage })));

export default function App() {
  return (
    <AuthProvider>  {/* Shared auth across all apps */}
      <BrowserRouter>
        <Layout>
          <Routes>
            <Route
              path="/video"
              element={
                <Suspense fallback={<div>Loading video app...</div>}>
                  <VideoPage />
                </Suspense>
              }
            />
            <Route
              path="/checkout"
              element={
                <Suspense fallback={<div>Loading checkout...</div>}>
                  <CheckoutPage />
                </Suspense>
              }
            />
            <Route
              path="/billing"
              element={
                <Suspense fallback={<div>Loading billing...</div>}>
                  <BillingPage />
                </Suspense>
              }
            />
          </Routes>
        </Layout>
      </BrowserRouter>
    </AuthProvider>
  );
}
```

## Shell App Layout

```typescript
// shell-app/src/Layout.tsx
import React from 'react';
import { Link } from 'react-router-dom';
import { useAuth } from 'shared-auth';

export default function Layout({ children }: { children: React.ReactNode }) {
  const { user, logout } = useAuth();

  return (
    <div className="shell-layout">
      <header className="shell-header">
        <nav>
          <Link to="/video">Video</Link>
          <Link to="/checkout">Checkout</Link>
          <Link to="/billing">Billing</Link>
        </nav>
        {user && (
          <div className="user-info">
            <span>{user.email}</span>
            <button onClick={logout}>Logout</button>
          </div>
        )}
      </header>

      <main className="shell-main">
        {children}  {/* Remote app renders here */}
      </main>

      <footer className="shell-footer">
        © 2024 DirecTV
      </footer>
    </div>
  );
}
```

---

## Remote App: Video (`apps/video/src/VideoPlayer.tsx`)

```typescript
// apps/video/src/VideoPlayer.tsx
import React from 'react';
import { useAuth } from 'shared-auth';

export default function VideoPage() {
  const { user } = useAuth();  // Access Shell's auth!

  if (!user) {
    return <div>Please log in to watch videos</div>;
  }

  return (
    <div className="video-app">
      <h1>Video Player</h1>
      <p>Logged in as: {user.email}</p>
      <video controls width="100%">
        <source src="/sample-video.mp4" type="video/mp4" />
      </video>
    </div>
  );
}
```

---

## Remote App: Checkout (`apps/checkout/src/Checkout.tsx`)

```typescript
// apps/checkout/src/Checkout.tsx
import React, { useState } from 'react';
import { useAuth } from 'shared-auth';
import Cart from './components/Cart';
import Payment from './components/Payment';

export default function CheckoutPage() {
  const { user, getAuthToken } = useAuth();  // Access Shell's auth!
  const [cartItems, setCartItems] = useState([]);
  const [step, setStep] = useState('cart'); // 'cart' or 'payment'

  if (!user) {
    return <div>Please log in to checkout</div>;
  }

  const handleCheckout = async () => {
    // Call backend API with JWT token from Shell's auth
    const response = await fetch('http://api.internal/checkout', {
      method: 'POST',
      headers: {
        'Authorization': `Bearer ${getAuthToken()}`,
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({ items: cartItems }),
    });
    // ...
  };

  return (
    <div className="checkout-app">
      <h1>Checkout</h1>
      {step === 'cart' && (
        <Cart
          items={cartItems}
          onNext={() => setStep('payment')}
        />
      )}
      {step === 'payment' && (
        <Payment
          onCheckout={handleCheckout}
        />
      )}
    </div>
  );
}
```

---

# PART 7 — Shared Authentication Across Apps

## Shared Auth Package (`packages/shared-auth/src/AuthProvider.tsx`)

```typescript
// packages/shared-auth/src/AuthProvider.tsx
import React, { createContext, useContext, useEffect, useState } from 'react';

interface User {
  userId: string;
  email: string;
  roles: string[];
}

interface AuthContextType {
  user: User | null;
  loading: boolean;
  login: (email: string, password: string) => Promise<void>;
  logout: () => Promise<void>;
  getAuthToken: () => string | null;
}

const AuthContext = createContext<AuthContextType | null>(null);

export function AuthProvider({ children }: { children: React.ReactNode }) {
  const [user, setUser] = useState<User | null>(null);
  const [loading, setLoading] = useState(true);
  const [token, setToken] = useState<string | null>(null);

  // Initialize on mount
  useEffect(() => {
    const initAuth = async () => {
      try {
        // Check if user already logged in (cookie exists)
        const response = await fetch('http://auth-service.internal/session', {
          credentials: 'include',
        });

        if (response.ok) {
          const data = await response.json();
          setUser(data.user);
          setToken(data.accessToken);
        }
      } catch (error) {
        console.error('Auth init failed:', error);
      } finally {
        setLoading(false);
      }
    };

    initAuth();
  }, []);

  const login = async (email: string, password: string) => {
    const response = await fetch('http://auth-service.internal/auth/login', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      credentials: 'include',
      body: JSON.stringify({ email, password }),
    });

    if (!response.ok) throw new Error('Login failed');

    const data = await response.json();
    setUser(data.user);
    setToken(data.accessToken);
  };

  const logout = async () => {
    await fetch('http://auth-service.internal/auth/logout', {
      method: 'POST',
      credentials: 'include',
    });

    setUser(null);
    setToken(null);
  };

  const getAuthToken = () => token;

  return (
    <AuthContext.Provider
      value={{ user, loading, login, logout, getAuthToken }}
    >
      {children}
    </AuthContext.Provider>
  );
}

export function useAuth() {
  const context = useContext(AuthContext);
  if (!context) {
    throw new Error('useAuth must be used within AuthProvider');
  }
  return context;
}
```

---

# PART 8 — Build and Deploy Pipeline

## Build Orchestration Script (`build.sh`)

```bash
#!/bin/bash

# build.sh - Build all micro-frontend apps independently

set -e  # Exit on error

echo "🏗️  Building Micro-Frontends..."

# Build Shell App
echo "📦 Building Shell App..."
cd shell-app
npm run build
echo "✅ Shell App built"
cd ..

# Build Video App (parallel)
echo "📦 Building Video App..."
(cd apps/video && npm run build) &
video_pid=$!

# Build Checkout App (parallel)
echo "📦 Building Checkout App..."
(cd apps/checkout && npm run build) &
checkout_pid=$!

# Build Billing App (parallel)
echo "📦 Building Billing App..."
(cd apps/billing && npm run build) &
billing_pid=$!

# Wait for all parallel builds
wait $video_pid $checkout_pid $billing_pid

echo "✅ All apps built successfully!"

# Report build outputs
echo ""
echo "📁 Build Outputs:"
echo "├── shell-app/dist/"
echo "├── apps/video/dist/"
echo "├── apps/checkout/dist/"
echo "└── apps/billing/dist/"
```

---

## GitHub Actions Deploy Pipeline

### `.github/workflows/shell.yml`

```yaml
name: Deploy Shell App

on:
  push:
    branches: [main]
    paths:
      - 'shell-app/**'
      - 'packages/shared-auth/**'
      - '.github/workflows/shell.yml'

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - uses: actions/setup-node@v3
        with:
          node-version: '18'

      - name: Install dependencies
        run: cd shell-app && npm ci

      - name: Build Shell App
        run: cd shell-app && npm run build

      - name: Upload to CDN
        env:
          CDN_TOKEN: ${{ secrets.CDN_TOKEN }}
        run: |
          cd shell-app/dist
          curl -X POST https://cdn.example.com/upload \
            -H "Authorization: Bearer $CDN_TOKEN" \
            -F "files=@shell.js" \
            -F "files=@remoteEntry.js"

      - name: Purge CDN Cache
        run: |
          curl -X POST https://cdn.example.com/purge \
            -H "Authorization: Bearer ${{ secrets.CDN_TOKEN }}" \
            -d "path=/shell/*"
```

### `.github/workflows/video.yml`

```yaml
name: Deploy Video App

on:
  push:
    branches: [main]
    paths:
      - 'apps/video/**'
      - 'packages/shared-auth/**'
      - '.github/workflows/video.yml'

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - uses: actions/setup-node@v3
        with:
          node-version: '18'

      - name: Install dependencies
        run: cd apps/video && npm ci

      - name: Build Video App
        run: cd apps/video && npm run build

      - name: Upload to CDN
        env:
          CDN_TOKEN: ${{ secrets.CDN_TOKEN }}
        run: |
          cd apps/video/dist
          curl -X POST https://cdn.example.com/upload \
            -H "Authorization: Bearer $CDN_TOKEN" \
            -F "files=@video.js" \
            -F "files=@remoteEntry.js"

      - name: Purge CDN Cache
        run: |
          curl -X POST https://cdn.example.com/purge \
            -H "Authorization: Bearer ${{ secrets.CDN_TOKEN }}" \
            -d "path=/video/*"
```

Similar for `checkout.yml` and `billing.yml`

---

# PART 9 — App Loading Timeline

## Complete User Journey

```
User visits: https://example.com

Timeline:

1. Browser loads Shell App
   ├── Downloads shell.js (100kb)
   ├── Downloads shell/remoteEntry.js
   └── Shell renders header + navigation

2. User clicks "Watch Video"
   ├── Shell sees route /video
   └── Lazy load <VideoPage />

3. Browser detects module federation import
   ├── Fetches https://cdn.example.com/video/remoteEntry.js
   ├── Caches it (won't fetch again unless cleared)
   └── Loads VideoPlayer module

4. Video app mounts
   ├── Accesses shared React from Shell
   ├── Accesses shared AuthContext from Shell
   ├── Can call Shell's useAuth() hook
   └── Renders video player

5. User watches video
   ├── Video app is fully loaded
   ├── Any auth checks use Shell's context
   └── Works seamlessly

6. User clicks "Go to Checkout"
   ├── Shell transitions to /checkout
   └── Lazy load <CheckoutPage />

7. Browser detects module federation import
   ├── Fetches https://cdn.example.com/checkout/remoteEntry.js
   ├── Caches it
   └── Loads Checkout module

8. Checkout app mounts
   ├── Same auth context from Shell
   ├── Displays cart
   └── Proceeds to payment

9. User logs out from Billing app
   ├── Calls logout() from AuthProvider
   ├── All apps lose authentication state
   ├── Shell redirects to login
   └── Video and Checkout apps also redirect
```

---

# PART 10 — Build Time Comparison

## Turborepo (Monorepo Build)

```
turbo run build

├── 1. Check cache (1s)
├── 2. Build packages/shared-auth (30s)
├── 3. Build apps/web (45s)
├── 4. Build apps/checkout (40s)
├── 5. Build apps/admin (35s)
└── Total: ~3-5 minutes

Output: All apps bundled together
Deploy: One deployment command
```

---

## Micro-Frontends (Independent Builds)

```
Scenario 1: Sequential build (developer runs manually)
$ cd shell && npm run build (45s)
$ cd apps/video && npm run build (30s)
$ cd apps/checkout && npm run build (35s)
$ cd apps/billing && npm run build (28s)
Total: ~2.5 minutes

Scenario 2: Parallel build (CI/CD)
Shell build: 45s ─┐
Video build: 30s  ├─ Run in parallel
Checkout:    35s  │
Billing:     28s ─┘
Total: ~45s (longest app wins!)

Scenario 3: Only Video changes (CI/CD)
Shell build: skipped
Video build: 30s ✅
Checkout:    skipped
Billing:     skipped
Total: ~30s (deploy only video app!)
```

---

# PART 11 — Shared Code in Micro-Frontends

## Shared Auth Package

```
packages/shared-auth/
├── src/
│   ├── AuthProvider.tsx  (Wraps Shell & all remote apps)
│   ├── useAuth.ts        (Hook used everywhere)
│   ├── getSession.ts     (Get JWT token)
│   └── api.ts            (Call auth service)
└── package.json          (Published to npm or used locally)

Used by:
├── shell-app (AuthProvider wraps whole app)
├── apps/video (imports useAuth)
├── apps/checkout (imports useAuth)
└── apps/billing (imports useAuth)
```

## Shared Utils Package

```
packages/shared-utils/
├── src/
│   ├── api.ts            (API client with JWT injection)
│   ├── logger.ts         (Centralized logging)
│   ├── formatters.ts     (Format date, currency, etc)
│   └── constants.ts      (Shared constants)
└── package.json

Used by all apps for consistency
```

---

# PART 12 — Advantages of Micro-Frontends

| Advantage                  | Explanation                                                                  |
| -------------------------- | ---------------------------------------------------------------------------- |
| **Independent Deployment** | Video team deploys without waiting for checkout team                         |
| **Team Autonomy**          | Each team owns their app entirely — independent tech choices (within reason) |
| **Faster Builds**          | Each app builds in parallel (45s total vs 5 min sequential)                  |
| **Selective Updates**      | Update only the app that changed, not entire monorepo                        |
| **Scalability**            | Add new apps without slowing down existing builds                            |
| **Fault Isolation**        | One app crashes, others still work (with error boundaries)                   |
| **Code Reuse**             | Share auth, utils, components via packages                                   |
| **Performance**            | Load-on-demand via dynamic imports (code splitting)                          |

---

# PART 13 — Disadvantages of Micro-Frontends

| Disadvantage                | Impact                                                     |
| --------------------------- | ---------------------------------------------------------- |
| **Complexity**              | 4-5 separate webpack configs, CI/CD pipelines              |
| **Shared State Management** | Context/Redux must work across dynamically loaded apps     |
| **Dependency Conflicts**    | What if video wants React 18.0, checkout wants React 19.0? |
| **Testing**                 | E2E tests must work with all apps loaded dynamically       |
| **Debugging**               | Harder to trace issues across multiple bundled apps        |
| **Bundle Size**             | If not shared properly, duplicate dependencies             |
| **Initial Page Load**       | Shell + first app load slower than single monolithic app   |
| **Team Coordination**       | Must agree on shared packages, auth, API contracts         |

---

# PART 14 — When to Use Micro-Frontends

### ✅ Use Micro-Frontends If:

- You have **multiple independent teams** (each owns one app)
- Teams need to **deploy independently** without coordination
- You want **different tech stacks** for different apps (React for Video, Vue for Billing)
- You have **large, complex frontend** (100+ developers)
- Build times are **critical bottleneck** (saves 80% time with parallel builds)
- You need **fault isolation** (one app crash doesn't crash others)

### ❌ Don't Use Micro-Frontends If:

- **Single team** managing all code (Turborepo is simpler)
- **Build time is not an issue** (app builds in < 2 min)
- Frequent **shared code changes** (micro-frontends make this harder)
- **Small team** (complexity not worth it)
- Need **tight integration** between all features
- **Learning curve** is a concern (webpack config is complex)

---

# PART 15 — DirecTV Use Case

## Recommended: Hybrid Approach

```
Shell App (Micro-frontend Container)
    ↓
├── apps/overview (Next.js with Turborepo)
│   ├── Dashboard
│   ├── Account
│   └── Settings
│
├── apps/video (Module Federation remote)
│   ├── Video player
│   ├── Streaming
│   └── DVR controls
│   (Built independently)
│
├── apps/checkout (Module Federation remote)
│   ├── Billing
│   ├── PPV
│   └── Subscription
│   (Built independently)
│
└── apps/admin (Module Federation remote)
    ├── User management
    ├── Content management
    └── Reports
    (Built independently)

Shared:
├── packages/shared-auth (used by all)
├── packages/shared-api (used by all)
└── packages/shared-ui (used by all)
```

---

# PART 16 — Key Takeaways

1. **Micro-frontends** = independent React apps loaded dynamically at runtime
2. **Turborepo** = multiple apps built together (monorepo), deploy together
3. **Module Federation** = webpack feature for runtime code sharing
4. **remoteEntry.js** = entry point for each micro-frontend app
5. **Singleton shared deps** = React, auth, utils loaded only once (huge perf win)
6. **Independent builds** = each app builds in parallel, faster total time
7. **Independent deploys** = deploy video without redeploying checkout
8. **Shared auth** = AuthProvider in Shell wraps all apps, they use useAuth()
9. **Dynamic imports** = `lazy(() => import('video/VideoPlayer'))` loads at runtime
10. **Error boundaries** = one app crash doesn't crash Shell or other apps
11. **Complexity trade-off** = More powerful but harder to manage
12. **Team structure drives architecture** = Multiple teams → micro-frontends; one team → Turborepo

---

## Summary

**Micro-Frontends Architecture** enables:

- Independent deployment of React applications
- Parallel builds (huge time savings)
- Autonomous teams with different tech stacks
- Code reuse via shared packages
- Fault isolation (one app error doesn't break others)
- Runtime dynamic loading via Module Federation

**Best used when:**

- Multiple teams building different features
- Fast deployment cycles are critical
- Teams need complete autonomy
- Code reuse is managed via shared packages

**Authentication across micro-frontends:**

- Shared AuthProvider wraps Shell app
- All remote apps access auth via useAuth() hook
- JWT token from auth service stored in shared context
- Logout affects all apps simultaneously
- Middleware/guards protect routes in each app
