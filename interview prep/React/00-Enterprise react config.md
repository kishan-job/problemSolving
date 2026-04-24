# Enterprise React App Configuration
## Complete Guide — From Zero to Production-Grade Setup

> **What this covers:**
> Everything that goes into a real company's React codebase beyond writing components.
> Build tools, code quality, testing, CI/CD, security, and deployment.
> Every section includes what it is, why it exists, and the exact configuration.

---

# Table of Contents

- [The Complete Map](#the-complete-map)
- [Section 1 — Project Scaffolding](#section-1--project-scaffolding)
- [Section 2 — TypeScript Configuration](#section-2--typescript-configuration)
- [Section 3 — Build Tools](#section-3--build-tools)
- [Section 4 — ESLint](#section-4--eslint)
- [Section 5 — Prettier](#section-5--prettier)
- [Section 6 — Stylelint](#section-6--stylelint)
- [Section 7 — Git Hooks with Husky and lint-staged](#section-7--git-hooks-with-husky-and-lint-staged)
- [Section 8 — Commit Message Standards with Commitlint](#section-8--commit-message-standards-with-commitlint)
- [Section 9 — Path Aliases](#section-9--path-aliases)
- [Section 10 — Environment Variables](#section-10--environment-variables)
- [Section 11 — Testing Setup](#section-11--testing-setup)
- [Section 12 — Folder Structure](#section-12--folder-structure)
- [Section 13 — CSS Architecture](#section-13--css-architecture)
- [Section 14 — State Management Setup](#section-14--state-management-setup)
- [Section 15 — API Layer Setup](#section-15--api-layer-setup)
- [Section 16 — Error Handling](#section-16--error-handling)
- [Section 17 — Performance Configuration](#section-17--performance-configuration)
- [Section 18 — Security Configuration](#section-18--security-configuration)
- [Section 19 — Bundle Analysis](#section-19--bundle-analysis)
- [Section 20 — CI/CD Pipeline](#section-20--cicd-pipeline)
- [Section 21 — Docker Setup](#section-21--docker-setup)
- [Section 22 — Monorepo Setup](#section-22--monorepo-setup)
- [Section 23 — VS Code Setup](#section-23--vs-code-setup)
- [Section 24 — Documentation Setup](#section-24--documentation-setup)
- [Section 25 — Complete Setup Checklist](#section-25--complete-setup-checklist)

---

# The Complete Map

> Everything that exists in enterprise React configuration.
> This is the map before the guide.

```
LAYER 1 — PROJECT FOUNDATION
  Project scaffolding (Vite vs CRA vs custom)
  TypeScript configuration (tsconfig.json)
  Package manager choice (npm vs yarn vs pnpm)
  Node version management (.nvmrc, .node-version)
  Git initialization and .gitignore

LAYER 2 — CODE QUALITY
  ESLint (catch errors and enforce patterns)
  Prettier (format code automatically)
  Stylelint (lint CSS/SCSS)
  EditorConfig (consistent editor settings)

LAYER 3 — GIT WORKFLOW
  Husky (run scripts on git events)
  lint-staged (run linters only on staged files)
  Commitlint (enforce commit message format)
  Conventional Commits standard

LAYER 4 — BUILD CONFIGURATION
  Vite configuration (plugins, optimizations)
  Build targets (browser support)
  Asset handling (images, fonts, SVGs)
  Code splitting configuration
  Source maps

LAYER 5 — ENVIRONMENT AND SECRETS
  .env files (.env, .env.local, .env.production)
  Environment variable validation (Zod)
  Secrets management strategy

LAYER 6 — TESTING
  Unit testing (Vitest)
  Component testing (React Testing Library)
  E2E testing (Playwright or Cypress)
  Test coverage configuration
  Mock service worker (MSW)

LAYER 7 — PROJECT STRUCTURE
  Folder structure (feature-based vs type-based)
  Path aliases (@ imports)
  Barrel files (index.ts exports)
  Module boundaries

LAYER 8 — CSS AND STYLING
  CSS architecture (Tailwind vs CSS Modules vs styled-components)
  Design tokens
  CSS reset/normalize
  Responsive system

LAYER 9 — APPLICATION ARCHITECTURE
  State management setup (Zustand or Redux Toolkit)
  API layer setup (TanStack Query + Axios)
  Error boundaries
  Routing setup (React Router)
  Authentication setup

LAYER 10 — PERFORMANCE
  Bundle analysis (rollup-plugin-visualizer)
  Lazy loading configuration
  Image optimization
  Web Vitals monitoring

LAYER 11 — SECURITY
  Content Security Policy
  Dependency auditing
  HTTPS enforcement
  XSS prevention

LAYER 12 — CI/CD AND DEPLOYMENT
  GitHub Actions pipeline
  Pre-merge checks
  Docker configuration
  Deployment targets (Vercel, AWS, Docker)

LAYER 13 — DEVELOPER EXPERIENCE
  VS Code settings and extensions
  Debug configuration
  Storybook (component documentation)
  Changelog automation
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section 1 — Project Scaffolding

## What It Is

How you create the initial project and which package manager you use. These decisions affect everything downstream.

## Package Manager — Use pnpm

```bash
# Why pnpm over npm/yarn:
# - 3x faster installs
# - Saves disk space (shared node_modules)
# - Strict about hoisting (catches real dependency issues)
# - Industry standard at many large companies

npm install -g pnpm
```

## Node Version Management

```bash
# .nvmrc — locks the Node version for the entire team
# Every developer and CI uses the same version

echo "20.11.0" > .nvmrc

# Developer runs:
nvm use
# CI runs:
# uses-node-version: .nvmrc in GitHub Actions
```

## Create Project with Vite

```bash
pnpm create vite my-app --template react-ts
cd my-app
pnpm install
```

## .gitignore for Enterprise React

```gitignore
# Dependencies
node_modules
.pnp
.pnp.js

# Build output
dist
dist-ssr
build
out

# Environment files — NEVER commit these
.env.local
.env.*.local
.env.production

# Testing
coverage
playwright-report
test-results

# Editor
.vscode/*
!.vscode/extensions.json
!.vscode/settings.json
!.vscode/launch.json
.idea
*.suo
*.ntvs*
*.njsproj
*.sln
*.sw?

# OS
.DS_Store
Thumbs.db

# Debug
npm-debug.log*
yarn-debug.log*
yarn-error.log*
pnpm-debug.log*

# Storybook
storybook-static

# Misc
*.local
```

## package.json Scripts (Complete Set)

```json
{
  "scripts": {
    "dev": "vite",
    "build": "tsc && vite build",
    "build:analyze": "ANALYZE=true vite build",
    "preview": "vite preview",
    "lint": "eslint . --ext ts,tsx --report-unused-disable-directives --max-warnings 0",
    "lint:fix": "eslint . --ext ts,tsx --fix",
    "format": "prettier --write .",
    "format:check": "prettier --check .",
    "type-check": "tsc --noEmit",
    "test": "vitest",
    "test:ui": "vitest --ui",
    "test:coverage": "vitest run --coverage",
    "test:e2e": "playwright test",
    "test:e2e:ui": "playwright test --ui",
    "prepare": "husky install",
    "storybook": "storybook dev -p 6006",
    "build-storybook": "storybook build"
  }
}
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section 2 — TypeScript Configuration

## What It Is

TypeScript configuration controls how strict the compiler is, which features are enabled, and how it resolves modules. Enterprise projects use strict mode — it catches real bugs at compile time.

## Why Strict Mode Matters

```
Without strict mode:
  function getUser(id) {        // ← id is any — could be anything
    return users[id].name       // ← crashes if user doesn't exist
  }

With strict mode:
  function getUser(id: string): User | undefined {  // ← forces you to handle undefined
    return users[id]
  }
  // Cannot call .name without checking if user exists — compiler catches the bug
```

## tsconfig.json — Production Configuration

```json
{
  "compilerOptions": {
    // === LANGUAGE AND ENVIRONMENT ===
    "target": "ES2020",
    "useDefineForClassFields": true,
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    "module": "ESNext",
    "skipLibCheck": true,

    // === MODULE RESOLUTION ===
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "react-jsx",

    // === STRICT TYPE CHECKING — ALL ENABLED ===
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true,
    "noImplicitReturns": true,
    "noImplicitOverride": true,

    // === PATH ALIASES ===
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"],
      "@components/*": ["src/components/*"],
      "@hooks/*": ["src/hooks/*"],
      "@utils/*": ["src/utils/*"],
      "@types/*": ["src/types/*"],
      "@assets/*": ["src/assets/*"],
      "@features/*": ["src/features/*"],
      "@store/*": ["src/store/*"],
      "@api/*": ["src/api/*"]
    }
  },
  "include": ["src", "vite.config.ts"],
  "references": [{ "path": "./tsconfig.node.json" }]
}
```

## tsconfig.node.json

```json
{
  "compilerOptions": {
    "composite": true,
    "skipLibCheck": true,
    "module": "ESNext",
    "moduleResolution": "bundler",
    "allowSyntheticDefaultImports": true,
    "strict": true
  },
  "include": ["vite.config.ts"]
}
```

## Common TypeScript Patterns in Enterprise Code

```typescript
// ✅ Always type API responses
interface User {
  id: string
  name: string
  email: string
  role: 'admin' | 'user' | 'viewer'  // ← Union type, not string
  createdAt: string
}

// ✅ Utility types — use them constantly
type UpdateUserPayload = Partial<Omit<User, 'id' | 'createdAt'>>
type UserSummary = Pick<User, 'id' | 'name'>

// ✅ Discriminated unions for state
type AsyncState<T> =
  | { status: 'idle' }
  | { status: 'loading' }
  | { status: 'success'; data: T }
  | { status: 'error'; error: string }

// ✅ Generic components
interface TableProps<T> {
  data: T[]
  columns: Column<T>[]
  onRowClick?: (row: T) => void
}

// ✅ const assertions for config objects
const ROUTES = {
  HOME: '/',
  USERS: '/users',
  SETTINGS: '/settings',
} as const

type Route = typeof ROUTES[keyof typeof ROUTES]
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section 3 — Build Tools

## What It Is

The build tool compiles, bundles, and optimizes your code for production. Vite is the industry standard for React in 2026.

## vite.config.ts — Complete Enterprise Configuration

```typescript
import { defineConfig, loadEnv } from 'vite'
import react from '@vitejs/plugin-react'
import path from 'path'
import { visualizer } from 'rollup-plugin-visualizer'

export default defineConfig(({ command, mode }) => {
  // Load env variables based on mode
  const env = loadEnv(mode, process.cwd(), '')

  return {
    plugins: [
      react(),
      // Bundle visualizer — only in analyze mode
      process.env.ANALYZE === 'true' &&
        visualizer({
          open: true,
          gzipSize: true,
          brotliSize: true,
          filename: 'dist/bundle-report.html',
        }),
    ].filter(Boolean),

    // === PATH ALIASES — must match tsconfig paths ===
    resolve: {
      alias: {
        '@': path.resolve(__dirname, './src'),
        '@components': path.resolve(__dirname, './src/components'),
        '@hooks': path.resolve(__dirname, './src/hooks'),
        '@utils': path.resolve(__dirname, './src/utils'),
        '@types': path.resolve(__dirname, './src/types'),
        '@assets': path.resolve(__dirname, './src/assets'),
        '@features': path.resolve(__dirname, './src/features'),
        '@store': path.resolve(__dirname, './src/store'),
        '@api': path.resolve(__dirname, './src/api'),
      },
    },

    // === BUILD CONFIGURATION ===
    build: {
      // Target modern browsers — reduces bundle size
      target: 'es2020',

      // Enable source maps in production for debugging
      sourcemap: mode === 'staging' ? true : false,

      // Chunk size warning threshold
      chunkSizeWarningLimit: 1000,

      rollupOptions: {
        output: {
          // Manual code splitting — keeps vendor code separate
          manualChunks: {
            // React core — cached separately
            'vendor-react': ['react', 'react-dom', 'react-router-dom'],
            // UI library — cached separately
            'vendor-ui': ['@radix-ui/react-dialog', '@radix-ui/react-dropdown-menu'],
            // Data fetching — cached separately
            'vendor-query': ['@tanstack/react-query', 'axios'],
            // State management
            'vendor-state': ['zustand'],
          },
        },
      },
    },

    // === DEVELOPMENT SERVER ===
    server: {
      port: 3000,
      strictPort: true,  // Fail if port is taken
      // Proxy API calls to avoid CORS in development
      proxy: {
        '/api': {
          target: env.VITE_API_URL,
          changeOrigin: true,
          rewrite: (path) => path.replace(/^\/api/, ''),
        },
      },
    },

    // === TESTING ===
    test: {
      globals: true,
      environment: 'jsdom',
      setupFiles: './src/test/setup.ts',
      coverage: {
        reporter: ['text', 'json', 'html'],
        exclude: [
          'node_modules/',
          'src/test/',
          '**/*.stories.tsx',
          '**/*.d.ts',
        ],
      },
    },
  }
})
```

## Why Manual Chunks Matter

```
Without manual chunks:
  app.js = 2.3MB (React + UI + state + your code all bundled)
  User downloads 2.3MB every deployment

With manual chunks:
  vendor-react.js   = 130KB (never changes — cached forever)
  vendor-ui.js      = 240KB (rarely changes — cached long)
  vendor-query.js   = 45KB  (rarely changes)
  app.js            = 180KB (your code — changes each deploy)

User downloads 130+240+45KB once, then only 180KB on updates
Page load: significantly faster on repeat visits
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section 4 — ESLint

## What It Is

ESLint finds bugs and enforces patterns in your code before they reach production. In enterprise projects it is non-negotiable — it runs in CI and blocks merges on violations.

## Install Dependencies

```bash
pnpm add -D eslint \
  @typescript-eslint/parser \
  @typescript-eslint/eslint-plugin \
  eslint-plugin-react \
  eslint-plugin-react-hooks \
  eslint-plugin-react-refresh \
  eslint-plugin-import \
  eslint-plugin-jsx-a11y \
  eslint-plugin-tailwindcss \
  eslint-config-prettier
```

## .eslintrc.cjs — Complete Enterprise Configuration

```javascript
module.exports = {
  root: true,
  env: {
    browser: true,
    es2020: true,
    node: true,
  },
  parser: '@typescript-eslint/parser',
  parserOptions: {
    ecmaVersion: 'latest',
    sourceType: 'module',
    ecmaFeatures: { jsx: true },
    project: ['./tsconfig.json', './tsconfig.node.json'],
  },
  plugins: [
    '@typescript-eslint',
    'react',
    'react-hooks',
    'react-refresh',
    'import',
    'jsx-a11y',
  ],
  extends: [
    'eslint:recommended',
    'plugin:@typescript-eslint/recommended',
    'plugin:@typescript-eslint/recommended-requiring-type-checking',
    'plugin:react/recommended',
    'plugin:react/jsx-runtime',
    'plugin:react-hooks/recommended',
    'plugin:jsx-a11y/recommended',
    'plugin:import/recommended',
    'plugin:import/typescript',
    'prettier', // ← Always last — disables rules that conflict with Prettier
  ],
  rules: {
    // === TYPESCRIPT ===
    '@typescript-eslint/no-explicit-any': 'error',         // No any type
    '@typescript-eslint/explicit-function-return-type': 'off',
    '@typescript-eslint/no-unused-vars': ['error', {
      argsIgnorePattern: '^_',    // Allow _unusedParam pattern
      varsIgnorePattern: '^_',
    }],
    '@typescript-eslint/no-floating-promises': 'error',    // Must await promises
    '@typescript-eslint/no-misused-promises': 'error',

    // === REACT ===
    'react/prop-types': 'off',             // TypeScript handles this
    'react/display-name': 'off',
    'react-refresh/only-export-components': ['warn', {
      allowConstantExport: true,
    }],

    // === IMPORTS ===
    'import/order': ['error', {
      groups: [
        'builtin',    // Node.js built-ins
        'external',   // npm packages
        'internal',   // path aliases (@/...)
        'parent',     // ../
        'sibling',    // ./
        'index',      // ./index
        'type',       // type imports
      ],
      'newlines-between': 'always',
      alphabetize: { order: 'asc', caseInsensitive: true },
    }],
    'import/no-duplicates': 'error',
    'import/no-cycle': 'error',           // Catches circular imports

    // === GENERAL ===
    'no-console': ['warn', { allow: ['warn', 'error'] }],  // No console.log in production
    'no-debugger': 'error',
    'prefer-const': 'error',
    'no-var': 'error',
  },
  settings: {
    react: { version: 'detect' },
    'import/resolver': {
      typescript: {
        alwaysTryTypes: true,
        project: './tsconfig.json',
      },
    },
  },
  overrides: [
    // Relax rules for test files
    {
      files: ['**/*.test.ts', '**/*.test.tsx', '**/*.spec.ts'],
      rules: {
        '@typescript-eslint/no-explicit-any': 'off',
        'no-console': 'off',
      },
    },
    // Relax rules for config files
    {
      files: ['*.config.ts', '*.config.js', '*.config.cjs'],
      rules: {
        'import/no-default-export': 'off',
      },
    },
  ],
}
```

## .eslintignore

```
dist
build
node_modules
coverage
storybook-static
*.min.js
public
```

## What Each Plugin Catches

| Plugin | What It Catches |
|---|---|
| @typescript-eslint | TypeScript-specific errors — any type, unused vars, misused promises |
| react-hooks | Wrong hook usage — missing dependencies, conditional hooks |
| jsx-a11y | Accessibility issues — missing alt text, invalid ARIA |
| import/order | Disorganized imports — forces consistent ordering |
| import/no-cycle | Circular dependencies — catches architectural problems |

[↑ Back to Table of Contents](#table-of-contents)

---

# Section 5 — Prettier

## What It Is

Prettier automatically formats your code. It removes all debates about formatting — tabs vs spaces, semicolons, quote style — because the formatter decides and everyone uses it.

## Why Prettier Over Manual Formatting

```
Without Prettier:
  Developer A writes:   const x = {a:1,b:2}
  Developer B writes:   const x = { a: 1, b: 2 }
  PRs filled with formatting debates
  Code review focuses on style not logic

With Prettier:
  Both get auto-formatted to: const x = { a: 1, b: 2 }
  Code review focuses only on logic
  Zero formatting discussions
```

## Install

```bash
pnpm add -D prettier
```

## .prettierrc — Enterprise Configuration

```json
{
  "semi": false,
  "singleQuote": true,
  "jsxSingleQuote": false,
  "trailingComma": "all",
  "printWidth": 100,
  "tabWidth": 2,
  "useTabs": false,
  "bracketSpacing": true,
  "bracketSameLine": false,
  "arrowParens": "always",
  "endOfLine": "lf",
  "importOrder": [
    "^(react|react-dom)(.*)$",
    "<THIRD_PARTY_MODULES>",
    "^@/(.*)$",
    "^[./]"
  ],
  "importOrderSeparation": true,
  "importOrderSortSpecifiers": true,
  "plugins": ["@trivago/prettier-plugin-sort-imports"]
}
```

## .prettierignore

```
dist
build
node_modules
coverage
.env*
pnpm-lock.yaml
yarn.lock
package-lock.json
storybook-static
public
```

## EditorConfig — Consistent Editor Settings

```ini
# .editorconfig
# Forces all editors (VS Code, WebStorm, vim) to use same settings

root = true

[*]
indent_style = space
indent_size = 2
end_of_line = lf
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true

[*.md]
trim_trailing_whitespace = false

[*.{yml,yaml}]
indent_size = 2
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section 6 — Stylelint

## What It Is

Stylelint lints your CSS, SCSS, and CSS-in-JS. Catches errors and enforces consistent patterns in styles.

## Install

```bash
pnpm add -D stylelint stylelint-config-standard stylelint-config-tailwindcss
```

## .stylelintrc.json

```json
{
  "extends": [
    "stylelint-config-standard",
    "stylelint-config-tailwindcss"
  ],
  "rules": {
    "color-no-invalid-hex": true,
    "shorthand-property-no-redundant-values": true,
    "declaration-block-no-duplicate-properties": true,
    "no-duplicate-selectors": true,
    "unit-no-unknown": true,
    "property-no-unknown": true,
    "selector-pseudo-class-no-unknown": [true, {
      "ignorePseudoClasses": ["global", "local"]
    }]
  }
}
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section 7 — Git Hooks with Husky and lint-staged

## What It Is

Husky runs scripts automatically when you do git actions (commit, push). lint-staged runs linters only on files you are committing — not the entire codebase.

## Why This Matters

```
Without git hooks:
  Developer commits broken code
  CI catches it 5 minutes later
  Developer must fix and push again
  Slows down everyone

With git hooks:
  Pre-commit: ESLint + Prettier run on changed files
  Commit-msg: Commitlint validates message format
  Pre-push: TypeScript check + tests run
  Broken code NEVER reaches the repository
```

## Install

```bash
pnpm add -D husky lint-staged
npx husky install
```

## package.json addition

```json
{
  "scripts": {
    "prepare": "husky install"
  }
}
```

## Create Hooks

```bash
# Pre-commit hook — runs before every commit
npx husky add .husky/pre-commit "npx lint-staged"

# Commit-msg hook — validates commit message
npx husky add .husky/commit-msg "npx --no -- commitlint --edit ${1}"

# Pre-push hook — runs before pushing to remote
npx husky add .husky/pre-push "pnpm type-check && pnpm test run"
```

## .lintstagedrc.cjs

```javascript
module.exports = {
  // TypeScript and JavaScript files
  '**/*.{ts,tsx,js,jsx}': [
    'eslint --fix --max-warnings 0',
    'prettier --write',
  ],
  // CSS files
  '**/*.{css,scss}': [
    'stylelint --fix',
    'prettier --write',
  ],
  // JSON, Markdown, YAML
  '**/*.{json,md,yml,yaml}': [
    'prettier --write',
  ],
}
```

## What Runs When

```
git add .
git commit -m "feat: add user profile page"

RUNS:
  lint-staged:
    ESLint on changed .ts/.tsx files → fixes what it can, errors block commit
    Prettier on changed files → formats automatically
    Stylelint on changed .css files

  commitlint:
    Validates "feat: add user profile page" follows conventional commits

git push origin main

RUNS:
  TypeScript check → fails if any type errors
  Vitest → fails if any tests fail
  Only then does code reach remote
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section 8 — Commit Message Standards with Commitlint

## What It Is

Commitlint enforces a consistent commit message format. This enables automatic changelog generation and makes git history readable.

## Install

```bash
pnpm add -D @commitlint/cli @commitlint/config-conventional
```

## commitlint.config.cjs

```javascript
module.exports = {
  extends: ['@commitlint/config-conventional'],
  rules: {
    'type-enum': [
      2,
      'always',
      [
        'feat',     // New feature
        'fix',      // Bug fix
        'docs',     // Documentation only
        'style',    // Formatting, no logic change
        'refactor', // Refactoring, no feature/fix
        'perf',     // Performance improvement
        'test',     // Adding tests
        'build',    // Build system or dependencies
        'ci',       // CI/CD configuration
        'chore',    // Maintenance tasks
        'revert',   // Reverting a commit
      ],
    ],
    'type-case': [2, 'always', 'lower-case'],
    'type-empty': [2, 'never'],
    'scope-case': [2, 'always', 'lower-case'],
    'subject-empty': [2, 'never'],
    'subject-full-stop': [2, 'never', '.'],
    'header-max-length': [2, 'always', 100],
  },
}
```

## Commit Message Format

```
type(scope): subject

body (optional)

footer (optional)

EXAMPLES:
  feat(auth): add JWT refresh token logic
  fix(user-profile): resolve avatar upload failing on mobile
  docs(readme): update setup instructions for pnpm
  refactor(api): extract error handling into shared utility
  test(auth): add unit tests for token validation
  chore(deps): update react to 18.3.0
  perf(dashboard): virtualize large data table
  ci(github-actions): add staging deployment workflow

WRONG:
  Added some stuff          ← no type
  feat: Added some stuff    ← capital letter
  FIX: bug fix.             ← wrong format, ends with period
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section 9 — Path Aliases

## What It Is

Path aliases replace long relative imports with short clean imports. They must be configured in three places: tsconfig.json, vite.config.ts, and ESLint.

## The Problem They Solve

```typescript
// Without aliases — painful relative paths
import { Button } from '../../../components/ui/Button'
import { useAuth } from '../../../../hooks/useAuth'
import { formatDate } from '../../../../../utils/date'

// With aliases — clean, location-independent
import { Button } from '@/components/ui/Button'
import { useAuth } from '@/hooks/useAuth'
import { formatDate } from '@/utils/date'

// Moving a file breaks nothing — the alias always resolves to src/
```

## Configuration in Three Places

**tsconfig.json** (already shown in Section 2)

**vite.config.ts** (already shown in Section 3)

**Install vite-tsconfig-paths plugin** (simpler approach):

```bash
pnpm add -D vite-tsconfig-paths
```

```typescript
// vite.config.ts — simpler version using the plugin
import tsconfigPaths from 'vite-tsconfig-paths'

export default defineConfig({
  plugins: [
    react(),
    tsconfigPaths(), // ← Reads aliases from tsconfig automatically
  ],
})
```

With this plugin you only define aliases in tsconfig.json — Vite reads them automatically. No duplication.

[↑ Back to Table of Contents](#table-of-contents)

---

# Section 10 — Environment Variables

## What It Is

Environment variables hold configuration that changes between environments — API URLs, feature flags, analytics keys. They are never committed to git.

## File Structure

```
.env                  ← Default values (committed to git, no secrets)
.env.local            ← Local overrides (NOT committed, in .gitignore)
.env.development      ← Development-specific (committed, no secrets)
.env.staging          ← Staging environment (NOT committed)
.env.production       ← Production environment (NOT committed)
.env.example          ← Template showing what variables exist (committed)
```

## .env.example — Always Commit This

```bash
# API
VITE_API_URL=https://api.yourapp.com
VITE_API_TIMEOUT=10000

# Authentication
VITE_AUTH_DOMAIN=yourapp.auth0.com
VITE_AUTH_CLIENT_ID=

# Feature Flags
VITE_FEATURE_NEW_DASHBOARD=false
VITE_FEATURE_AI_ASSISTANT=false

# Analytics
VITE_ANALYTICS_ID=
VITE_SENTRY_DSN=

# App
VITE_APP_NAME=MyApp
VITE_APP_VERSION=$npm_package_version
```

## Validate Environment Variables with Zod

```typescript
// src/config/env.ts
// This runs at startup and crashes IMMEDIATELY if a required variable is missing
// Instead of mysterious errors later at runtime

import { z } from 'zod'

const envSchema = z.object({
  VITE_API_URL: z.string().url('VITE_API_URL must be a valid URL'),
  VITE_API_TIMEOUT: z.string().transform(Number).default('10000'),
  VITE_AUTH_DOMAIN: z.string().min(1, 'VITE_AUTH_DOMAIN is required'),
  VITE_AUTH_CLIENT_ID: z.string().min(1, 'VITE_AUTH_CLIENT_ID is required'),
  VITE_FEATURE_NEW_DASHBOARD: z
    .string()
    .transform((v) => v === 'true')
    .default('false'),
  VITE_APP_NAME: z.string().default('App'),
})

const parsed = envSchema.safeParse(import.meta.env)

if (!parsed.success) {
  console.error('❌ Invalid environment variables:')
  console.error(parsed.error.format())
  throw new Error('Invalid environment variables. Check console for details.')
}

export const env = parsed.data

// Usage anywhere in the app:
// import { env } from '@/config/env'
// env.VITE_API_URL — fully typed, validated at startup
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section 11 — Testing Setup

## What It Is

Enterprise projects have three types of tests: unit tests (functions), component tests (React components), and E2E tests (full user flows in a browser).

## Install

```bash
# Unit + component testing
pnpm add -D vitest @vitest/ui @vitest/coverage-v8 \
  @testing-library/react \
  @testing-library/jest-dom \
  @testing-library/user-event \
  jsdom

# API mocking
pnpm add -D msw

# E2E testing
pnpm add -D @playwright/test
npx playwright install
```

## Test Setup File

```typescript
// src/test/setup.ts
import '@testing-library/jest-dom'
import { afterEach, beforeAll, afterAll } from 'vitest'
import { cleanup } from '@testing-library/react'
import { server } from './mocks/server'

// Clean up DOM after each test
afterEach(() => {
  cleanup()
})

// Start MSW mock server
beforeAll(() => server.listen({ onUnhandledRequest: 'error' }))
afterEach(() => server.resetHandlers())
afterAll(() => server.close())
```

## MSW Mock Server Setup

```typescript
// src/test/mocks/handlers.ts
import { http, HttpResponse } from 'msw'

export const handlers = [
  http.get('/api/users', () => {
    return HttpResponse.json([
      { id: '1', name: 'Alice', email: 'alice@example.com' },
      { id: '2', name: 'Bob', email: 'bob@example.com' },
    ])
  }),

  http.post('/api/users', async ({ request }) => {
    const body = await request.json()
    return HttpResponse.json({ id: '3', ...body }, { status: 201 })
  }),

  http.get('/api/users/:id', ({ params }) => {
    if (params.id === '999') {
      return HttpResponse.json({ message: 'User not found' }, { status: 404 })
    }
    return HttpResponse.json({ id: params.id, name: 'Alice' })
  }),
]

// src/test/mocks/server.ts
import { setupServer } from 'msw/node'
import { handlers } from './handlers'

export const server = setupServer(...handlers)
```

## Component Test Example

```typescript
// src/features/users/UserCard.test.tsx
import { render, screen } from '@testing-library/react'
import userEvent from '@testing-library/user-event'
import { describe, it, expect, vi } from 'vitest'
import { UserCard } from './UserCard'

const mockUser = {
  id: '1',
  name: 'Alice Johnson',
  email: 'alice@example.com',
  role: 'admin' as const,
}

describe('UserCard', () => {
  it('renders user name and email', () => {
    render(<UserCard user={mockUser} />)

    expect(screen.getByText('Alice Johnson')).toBeInTheDocument()
    expect(screen.getByText('alice@example.com')).toBeInTheDocument()
  })

  it('calls onEdit when edit button is clicked', async () => {
    const onEdit = vi.fn()
    const user = userEvent.setup()

    render(<UserCard user={mockUser} onEdit={onEdit} />)

    await user.click(screen.getByRole('button', { name: /edit/i }))

    expect(onEdit).toHaveBeenCalledWith(mockUser)
    expect(onEdit).toHaveBeenCalledTimes(1)
  })

  it('shows admin badge for admin users', () => {
    render(<UserCard user={mockUser} />)
    expect(screen.getByText('Admin')).toBeInTheDocument()
  })
})
```

## Playwright E2E Test Example

```typescript
// e2e/auth.spec.ts
import { test, expect } from '@playwright/test'

test.describe('Authentication', () => {
  test('user can log in with valid credentials', async ({ page }) => {
    await page.goto('/login')

    await page.getByLabel('Email').fill('user@example.com')
    await page.getByLabel('Password').fill('password123')
    await page.getByRole('button', { name: 'Log in' }).click()

    await expect(page).toHaveURL('/dashboard')
    await expect(page.getByText('Welcome back')).toBeVisible()
  })

  test('shows error with invalid credentials', async ({ page }) => {
    await page.goto('/login')

    await page.getByLabel('Email').fill('wrong@example.com')
    await page.getByLabel('Password').fill('wrongpassword')
    await page.getByRole('button', { name: 'Log in' }).click()

    await expect(page.getByText('Invalid email or password')).toBeVisible()
    await expect(page).toHaveURL('/login')
  })
})
```

## Coverage Configuration

```typescript
// vitest.config.ts (or inside vite.config.ts test section)
export default {
  test: {
    coverage: {
      provider: 'v8',
      reporter: ['text', 'json', 'html', 'lcov'],
      // Thresholds — CI fails if coverage drops below these
      thresholds: {
        lines: 80,
        functions: 80,
        branches: 70,
        statements: 80,
      },
      exclude: [
        'node_modules/',
        'src/test/',
        '**/*.stories.tsx',
        '**/*.d.ts',
        'src/main.tsx',
        'src/vite-env.d.ts',
      ],
    },
  },
}
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section 12 — Folder Structure

## What It Is

How you organize files determines how the codebase scales. Enterprise projects use feature-based structure — everything related to a feature lives together.

## Feature-Based Structure (Recommended)

```
src/
├── assets/                    # Images, fonts, icons
│   ├── images/
│   ├── fonts/
│   └── icons/
│
├── components/                # Shared UI components (used across features)
│   ├── ui/                    # Base components (Button, Input, Modal)
│   │   ├── Button/
│   │   │   ├── Button.tsx
│   │   │   ├── Button.test.tsx
│   │   │   ├── Button.stories.tsx
│   │   │   └── index.ts       # Re-exports
│   │   ├── Input/
│   │   └── index.ts           # Barrel file for all UI components
│   └── layout/                # Layout components (Header, Sidebar, Footer)
│       ├── Header/
│       ├── Sidebar/
│       └── index.ts
│
├── features/                  # Feature modules — the core of the app
│   ├── auth/
│   │   ├── components/        # Components only used in auth feature
│   │   │   ├── LoginForm.tsx
│   │   │   └── index.ts
│   │   ├── hooks/             # Hooks only used in auth
│   │   │   ├── useAuth.ts
│   │   │   └── index.ts
│   │   ├── api/               # API calls for auth
│   │   │   ├── auth.api.ts
│   │   │   └── index.ts
│   │   ├── store/             # State for auth (Zustand slice)
│   │   │   └── auth.store.ts
│   │   ├── types/             # Types only used in auth
│   │   │   └── auth.types.ts
│   │   ├── utils/             # Utils only used in auth
│   │   │   └── token.utils.ts
│   │   └── index.ts           # Public API of this feature
│   │
│   ├── users/
│   │   ├── components/
│   │   ├── hooks/
│   │   ├── api/
│   │   ├── types/
│   │   └── index.ts
│   │
│   └── dashboard/
│
├── hooks/                     # Shared hooks (used across features)
│   ├── useDebounce.ts
│   ├── useLocalStorage.ts
│   ├── useMediaQuery.ts
│   └── index.ts
│
├── api/                       # Shared API configuration
│   ├── client.ts              # Axios instance setup
│   ├── endpoints.ts           # API endpoint constants
│   └── index.ts
│
├── store/                     # Global state (if needed)
│   ├── app.store.ts
│   └── index.ts
│
├── router/                    # Routing configuration
│   ├── routes.tsx
│   ├── ProtectedRoute.tsx
│   └── index.ts
│
├── types/                     # Shared TypeScript types
│   ├── api.types.ts
│   ├── common.types.ts
│   └── index.ts
│
├── utils/                     # Shared utility functions
│   ├── date.utils.ts
│   ├── format.utils.ts
│   ├── validation.utils.ts
│   └── index.ts
│
├── config/                    # App configuration
│   ├── env.ts                 # Environment variable validation
│   ├── constants.ts           # App-wide constants
│   └── index.ts
│
├── styles/                    # Global styles
│   ├── globals.css
│   ├── variables.css
│   └── reset.css
│
├── test/                      # Test utilities and mocks
│   ├── setup.ts
│   ├── utils.tsx              # Custom render with providers
│   └── mocks/
│       ├── handlers.ts
│       └── server.ts
│
├── App.tsx
└── main.tsx
```

## Barrel Files (index.ts) — The Key Pattern

```typescript
// src/components/ui/index.ts
// Export everything from one place
// Consumers import from '@/components/ui' not individual files

export { Button } from './Button/Button'
export { Input } from './Input/Input'
export { Modal } from './Modal/Modal'
export { Spinner } from './Spinner/Spinner'
export type { ButtonProps } from './Button/Button'
export type { InputProps } from './Input/Input'

// Usage:
import { Button, Input, Modal } from '@/components/ui'
// Instead of:
import { Button } from '@/components/ui/Button/Button'
import { Input } from '@/components/ui/Input/Input'
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section 13 — CSS Architecture

## What It Is

How styles are organized and applied. Three main approaches: Tailwind CSS (utility-first), CSS Modules (scoped CSS), and styled-components (CSS-in-JS). Enterprise projects typically choose one and standardize on it.

## Option 1 — Tailwind CSS (Recommended for 2026)

```bash
pnpm add -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

```javascript
// tailwind.config.ts
import type { Config } from 'tailwindcss'

const config: Config = {
  content: ['./index.html', './src/**/*.{ts,tsx}'],
  theme: {
    extend: {
      // Design tokens — your brand colors, spacing, fonts
      colors: {
        primary: {
          50: '#eff6ff',
          500: '#3b82f6',
          600: '#2563eb',
          900: '#1e3a8a',
        },
        gray: {
          50: '#f9fafb',
          // ... full scale
        },
      },
      fontFamily: {
        sans: ['Inter var', 'system-ui', 'sans-serif'],
        mono: ['JetBrains Mono', 'monospace'],
      },
      spacing: {
        18: '4.5rem',
        88: '22rem',
      },
      borderRadius: {
        '4xl': '2rem',
      },
    },
  },
  plugins: [
    require('@tailwindcss/forms'),
    require('@tailwindcss/typography'),
  ],
}

export default config
```

## Option 2 — CSS Modules (No external library needed)

```typescript
// UserCard.module.css
.card {
  padding: 1rem;
  border: 1px solid var(--color-border);
  border-radius: 0.5rem;
}

.card:hover {
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.title {
  font-size: 1.125rem;
  font-weight: 600;
  color: var(--color-text-primary);
}

// UserCard.tsx
import styles from './UserCard.module.css'

export function UserCard({ user }) {
  return (
    <div className={styles.card}>
      <h3 className={styles.title}>{user.name}</h3>
    </div>
  )
}
```

## CSS Variables for Design Tokens

```css
/* src/styles/variables.css */
:root {
  /* Colors */
  --color-primary: #3b82f6;
  --color-primary-dark: #2563eb;
  --color-text-primary: #111827;
  --color-text-secondary: #6b7280;
  --color-border: #e5e7eb;
  --color-background: #ffffff;
  --color-surface: #f9fafb;

  /* Typography */
  --font-size-sm: 0.875rem;
  --font-size-base: 1rem;
  --font-size-lg: 1.125rem;
  --font-size-xl: 1.25rem;

  /* Spacing */
  --spacing-1: 0.25rem;
  --spacing-2: 0.5rem;
  --spacing-4: 1rem;
  --spacing-8: 2rem;

  /* Shadows */
  --shadow-sm: 0 1px 2px rgba(0, 0, 0, 0.05);
  --shadow-md: 0 4px 6px rgba(0, 0, 0, 0.07);

  /* Transitions */
  --transition-fast: 150ms ease;
  --transition-normal: 250ms ease;
}

/* Dark mode */
[data-theme='dark'] {
  --color-text-primary: #f9fafb;
  --color-text-secondary: #9ca3af;
  --color-border: #374151;
  --color-background: #111827;
  --color-surface: #1f2937;
}
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section 14 — State Management Setup

## What It Is

How application data is stored and shared across components. Enterprise apps separate server state (from API) and client state (UI state).

## Server State — TanStack Query

```bash
pnpm add @tanstack/react-query @tanstack/react-query-devtools
```

```typescript
// src/main.tsx — Provider setup
import { QueryClient, QueryClientProvider } from '@tanstack/react-query'
import { ReactQueryDevtools } from '@tanstack/react-query-devtools'

const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      staleTime: 5 * 60 * 1000,     // Data fresh for 5 minutes
      gcTime: 10 * 60 * 1000,        // Keep in cache 10 minutes
      retry: 2,                       // Retry failed requests twice
      refetchOnWindowFocus: false,    // Don't refetch on tab switch
    },
    mutations: {
      retry: 0,                       // Don't retry mutations
    },
  },
})

root.render(
  <QueryClientProvider client={queryClient}>
    <App />
    <ReactQueryDevtools initialIsOpen={false} />
  </QueryClientProvider>
)
```

## Client State — Zustand

```bash
pnpm add zustand
```

```typescript
// src/store/app.store.ts
import { create } from 'zustand'
import { devtools, persist } from 'zustand/middleware'

interface AppState {
  theme: 'light' | 'dark'
  sidebarOpen: boolean
  setTheme: (theme: 'light' | 'dark') => void
  toggleSidebar: () => void
}

export const useAppStore = create<AppState>()(
  devtools(
    persist(
      (set) => ({
        theme: 'light',
        sidebarOpen: true,

        setTheme: (theme) => set({ theme }),
        toggleSidebar: () =>
          set((state) => ({ sidebarOpen: !state.sidebarOpen })),
      }),
      {
        name: 'app-storage',    // localStorage key
        partialize: (state) => ({ theme: state.theme }), // Only persist theme
      }
    )
  )
)
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section 15 — API Layer Setup

## What It Is

A centralized, typed API layer that all components use. Handles authentication headers, error responses, and timeouts consistently.

```bash
pnpm add axios
```

```typescript
// src/api/client.ts
import axios, { type AxiosError, type AxiosResponse } from 'axios'
import { env } from '@/config/env'

// Create axios instance with defaults
export const apiClient = axios.create({
  baseURL: env.VITE_API_URL,
  timeout: env.VITE_API_TIMEOUT,
  headers: {
    'Content-Type': 'application/json',
  },
})

// Request interceptor — adds auth token to every request
apiClient.interceptors.request.use(
  (config) => {
    const token = localStorage.getItem('auth_token')
    if (token) {
      config.headers.Authorization = `Bearer ${token}`
    }
    return config
  },
  (error) => Promise.reject(error)
)

// Response interceptor — handles errors globally
apiClient.interceptors.response.use(
  (response: AxiosResponse) => response,
  async (error: AxiosError) => {
    if (error.response?.status === 401) {
      // Token expired — clear auth and redirect to login
      localStorage.removeItem('auth_token')
      window.location.href = '/login'
    }

    if (error.response?.status === 403) {
      // Forbidden — redirect to unauthorized page
      window.location.href = '/unauthorized'
    }

    return Promise.reject(error)
  }
)

// src/features/users/api/users.api.ts
import { apiClient } from '@/api/client'
import type { User, UpdateUserPayload } from '../types/user.types'

export const usersApi = {
  getAll: () =>
    apiClient.get<User[]>('/users').then((r) => r.data),

  getById: (id: string) =>
    apiClient.get<User>(`/users/${id}`).then((r) => r.data),

  create: (data: Omit<User, 'id' | 'createdAt'>) =>
    apiClient.post<User>('/users', data).then((r) => r.data),

  update: (id: string, data: UpdateUserPayload) =>
    apiClient.patch<User>(`/users/${id}`, data).then((r) => r.data),

  delete: (id: string) =>
    apiClient.delete(`/users/${id}`),
}

// src/features/users/hooks/useUsers.ts
import { useQuery, useMutation, useQueryClient } from '@tanstack/react-query'
import { usersApi } from '../api/users.api'

export const userKeys = {
  all: ['users'] as const,
  lists: () => [...userKeys.all, 'list'] as const,
  detail: (id: string) => [...userKeys.all, 'detail', id] as const,
}

export function useUsers() {
  return useQuery({
    queryKey: userKeys.lists(),
    queryFn: usersApi.getAll,
  })
}

export function useUser(id: string) {
  return useQuery({
    queryKey: userKeys.detail(id),
    queryFn: () => usersApi.getById(id),
    enabled: !!id,
  })
}

export function useUpdateUser() {
  const queryClient = useQueryClient()

  return useMutation({
    mutationFn: ({ id, data }: { id: string; data: UpdateUserPayload }) =>
      usersApi.update(id, data),
    onSuccess: (_, { id }) => {
      // Invalidate cache — refetches user list and detail
      void queryClient.invalidateQueries({ queryKey: userKeys.all })
    },
  })
}
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section 16 — Error Handling

## What It Is

Enterprise apps handle errors at multiple levels. Error boundaries catch React rendering errors. Global handlers catch unhandled promises. API errors are handled centrally.

```typescript
// src/components/ui/ErrorBoundary/ErrorBoundary.tsx
import { Component, type ReactNode, type ErrorInfo } from 'react'

interface Props {
  children: ReactNode
  fallback?: ReactNode
  onError?: (error: Error, info: ErrorInfo) => void
}

interface State {
  hasError: boolean
  error?: Error
}

export class ErrorBoundary extends Component<Props, State> {
  constructor(props: Props) {
    super(props)
    this.state = { hasError: false }
  }

  static getDerivedStateFromError(error: Error): State {
    return { hasError: true, error }
  }

  componentDidCatch(error: Error, info: ErrorInfo) {
    // Log to error tracking (Sentry, Datadog)
    console.error('ErrorBoundary caught:', error, info)
    this.props.onError?.(error, info)
  }

  render() {
    if (this.state.hasError) {
      return this.props.fallback ?? (
        <div role="alert">
          <h2>Something went wrong</h2>
          <button onClick={() => this.setState({ hasError: false })}>
            Try again
          </button>
        </div>
      )
    }

    return this.props.children
  }
}

// src/main.tsx — wrap the entire app
root.render(
  <ErrorBoundary
    fallback={<div>Application error. Please refresh.</div>}
    onError={(error) => {
      // Send to Sentry or your error tracker
      // Sentry.captureException(error)
    }}
  >
    <QueryClientProvider client={queryClient}>
      <App />
    </QueryClientProvider>
  </ErrorBoundary>
)
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section 17 — Performance Configuration

## What It Is

Configuration that makes the app fast — lazy loading routes, optimized images, code splitting, and Web Vitals monitoring.

## Lazy Loading Routes

```typescript
// src/router/routes.tsx
import { lazy, Suspense } from 'react'
import { createBrowserRouter } from 'react-router-dom'
import { Spinner } from '@/components/ui'

// Each page loads only when navigated to
const Dashboard = lazy(() => import('@/features/dashboard/pages/DashboardPage'))
const Users = lazy(() => import('@/features/users/pages/UsersPage'))
const Settings = lazy(() => import('@/features/settings/pages/SettingsPage'))

export const router = createBrowserRouter([
  {
    path: '/',
    element: <RootLayout />,
    children: [
      {
        path: 'dashboard',
        element: (
          <Suspense fallback={<Spinner />}>
            <Dashboard />
          </Suspense>
        ),
      },
      {
        path: 'users',
        element: (
          <Suspense fallback={<Spinner />}>
            <Users />
          </Suspense>
        ),
      },
    ],
  },
])
```

## React.memo and useMemo — When to Use

```typescript
// Only memo components that:
// 1. Re-render often
// 2. Are expensive to render
// 3. Receive the same props frequently

const UserRow = memo(({ user, onSelect }: UserRowProps) => {
  return (
    <tr onClick={() => onSelect(user)}>
      <td>{user.name}</td>
      <td>{user.email}</td>
    </tr>
  )
})

// Only useMemo for expensive calculations
const filteredUsers = useMemo(
  () => users.filter((u) => u.name.toLowerCase().includes(searchQuery)),
  [users, searchQuery]
  // Only recalculates when users array or searchQuery changes
)

// Only useCallback when passing to memoized children
const handleSelect = useCallback((user: User) => {
  setSelectedUser(user)
}, []) // Empty deps — function never changes
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section 18 — Security Configuration

## What It Is

Security configuration prevents common web vulnerabilities — XSS, clickjacking, exposed sensitive data.

## Dependency Auditing

```bash
# Run regularly — catches known vulnerabilities in dependencies
pnpm audit

# Fix automatically where possible
pnpm audit --fix

# Add to CI pipeline — fails build if high severity vulnerabilities found
pnpm audit --audit-level high
```

## Security Headers (via Vite Plugin or Server Config)

```typescript
// vite.config.ts
import { defineConfig } from 'vite'

export default defineConfig({
  plugins: [
    {
      name: 'security-headers',
      configureServer(server) {
        server.middlewares.use((req, res, next) => {
          res.setHeader('X-Content-Type-Options', 'nosniff')
          res.setHeader('X-Frame-Options', 'DENY')
          res.setHeader('X-XSS-Protection', '1; mode=block')
          res.setHeader('Referrer-Policy', 'strict-origin-when-cross-origin')
          next()
        })
      },
    },
  ],
})
```

## What NOT to Do

```typescript
// ❌ NEVER — exposes secrets in frontend code
const API_SECRET = 'sk-prod-xxxx...'

// ❌ NEVER — bypasses XSS protection
element.innerHTML = userProvidedContent

// ❌ NEVER — commits secrets to git
// .env.production file with real keys in git history

// ❌ NEVER — logs sensitive data
console.log('User password:', password)
console.log('Auth token:', token)

// ✅ DO — use environment variables for non-secret config
const API_URL = env.VITE_API_URL   // ← Validated, typed, no secrets

// ✅ DO — use React's JSX for safe rendering
<div>{userProvidedContent}</div>   // ← React escapes this automatically
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section 19 — Bundle Analysis

## What It Is

Bundle analysis shows you what is inside your production build, how large each piece is, and what you can reduce.

## Setup

```bash
pnpm add -D rollup-plugin-visualizer
```

```typescript
// vite.config.ts
import { visualizer } from 'rollup-plugin-visualizer'

export default defineConfig({
  plugins: [
    process.env.ANALYZE === 'true' &&
      visualizer({
        open: true,
        gzipSize: true,
        brotliSize: true,
        filename: 'dist/bundle-report.html',
      }),
  ].filter(Boolean),
})
```

```bash
# Run bundle analysis
pnpm build:analyze
# Opens interactive treemap showing every package and its size
```

## What to Look For

```
Warning signs in bundle report:
  - node_modules taking >70% of bundle (too many dependencies)
  - Same library imported multiple times (duplicate dependencies)
  - A single vendor chunk >500KB (needs splitting)
  - Moment.js or lodash (replace with date-fns and native methods)
  - Development-only packages in production build

Good bundle:
  - vendor chunks cached separately
  - app code <200KB gzipped
  - Each route chunk lazy loaded
  - No duplicate dependencies
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section 20 — CI/CD Pipeline

## What It Is

CI/CD runs your quality checks automatically on every pull request and deploys automatically when code merges to main.

## .github/workflows/ci.yml — Complete Pipeline

```yaml
name: CI

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main, develop]

jobs:
  quality:
    name: Code Quality
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version-file: '.nvmrc'

      - name: Setup pnpm
        uses: pnpm/action-setup@v3
        with:
          version: 8

      - name: Get pnpm store directory
        id: pnpm-cache
        run: echo "STORE_PATH=$(pnpm store path)" >> $GITHUB_OUTPUT

      - name: Cache pnpm modules
        uses: actions/cache@v4
        with:
          path: ${{ steps.pnpm-cache.outputs.STORE_PATH }}
          key: ${{ runner.os }}-pnpm-${{ hashFiles('**/pnpm-lock.yaml') }}
          restore-keys: ${{ runner.os }}-pnpm-

      - name: Install dependencies
        run: pnpm install --frozen-lockfile

      - name: TypeScript check
        run: pnpm type-check

      - name: ESLint
        run: pnpm lint

      - name: Prettier check
        run: pnpm format:check

      - name: Security audit
        run: pnpm audit --audit-level high

  test:
    name: Tests
    runs-on: ubuntu-latest
    needs: quality

    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version-file: '.nvmrc'
      - uses: pnpm/action-setup@v3
        with:
          version: 8
      - run: pnpm install --frozen-lockfile

      - name: Unit and component tests
        run: pnpm test:coverage

      - name: Upload coverage
        uses: codecov/codecov-action@v4
        with:
          token: ${{ secrets.CODECOV_TOKEN }}

  build:
    name: Build
    runs-on: ubuntu-latest
    needs: test

    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version-file: '.nvmrc'
      - uses: pnpm/action-setup@v3
        with:
          version: 8
      - run: pnpm install --frozen-lockfile

      - name: Build
        run: pnpm build
        env:
          VITE_API_URL: ${{ secrets.VITE_API_URL }}
          VITE_AUTH_CLIENT_ID: ${{ secrets.VITE_AUTH_CLIENT_ID }}

      - name: Upload build artifacts
        uses: actions/upload-artifact@v4
        with:
          name: dist
          path: dist/
          retention-days: 1

  e2e:
    name: E2E Tests
    runs-on: ubuntu-latest
    needs: build

    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version-file: '.nvmrc'
      - uses: pnpm/action-setup@v3
        with:
          version: 8
      - run: pnpm install --frozen-lockfile
      - run: npx playwright install --with-deps

      - name: Download build
        uses: actions/download-artifact@v4
        with:
          name: dist
          path: dist/

      - name: E2E tests
        run: pnpm test:e2e

      - name: Upload Playwright report
        uses: actions/upload-artifact@v4
        if: always()
        with:
          name: playwright-report
          path: playwright-report/
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section 21 — Docker Setup

## What It Is

Docker packages your app into a container that runs identically everywhere — developer machine, staging, production.

## Dockerfile — Multi-Stage Build

```dockerfile
# Stage 1 — Dependencies
FROM node:20-alpine AS deps
WORKDIR /app

# Install pnpm
RUN npm install -g pnpm

# Copy package files
COPY package.json pnpm-lock.yaml ./

# Install dependencies
RUN pnpm install --frozen-lockfile

# ─────────────────────────────────────────────

# Stage 2 — Build
FROM node:20-alpine AS builder
WORKDIR /app

RUN npm install -g pnpm

# Copy deps from previous stage
COPY --from=deps /app/node_modules ./node_modules
COPY . .

# Build arguments for environment variables
ARG VITE_API_URL
ARG VITE_AUTH_CLIENT_ID
ENV VITE_API_URL=$VITE_API_URL
ENV VITE_AUTH_CLIENT_ID=$VITE_AUTH_CLIENT_ID

RUN pnpm build

# ─────────────────────────────────────────────

# Stage 3 — Production server (nginx)
FROM nginx:alpine AS runner

# Copy nginx configuration
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Copy built files
COPY --from=builder /app/dist /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

## nginx.conf

```nginx
server {
    listen 80;
    server_name _;
    root /usr/share/nginx/html;
    index index.html;

    # Gzip compression
    gzip on;
    gzip_types text/plain text/css application/json
               application/javascript text/xml application/xml
               application/xml+rss text/javascript;

    # Cache static assets aggressively
    location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg|woff|woff2)$ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }

    # SPA routing — all routes serve index.html
    location / {
        try_files $uri $uri/ /index.html;
    }

    # Security headers
    add_header X-Frame-Options "DENY" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header X-XSS-Protection "1; mode=block" always;
}
```

## .dockerignore

```
node_modules
dist
.git
.gitignore
*.md
.env*
coverage
playwright-report
storybook-static
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section 22 — Monorepo Setup

## What It Is

A monorepo puts multiple related packages in one repository — frontend app, shared UI library, shared utilities, backend. Used at larger companies when multiple teams share code.

## When to Use a Monorepo

```
Use monorepo when:
  - Multiple React apps share the same UI components
  - Frontend and backend share TypeScript types
  - Multiple teams work in the same codebase
  - You want unified versioning and testing

Do NOT use monorepo when:
  - Single app, single team
  - Just starting out
  - Teams have completely separate deployments
```

## pnpm Workspaces Setup

```yaml
# pnpm-workspace.yaml
packages:
  - 'apps/*'      # React applications
  - 'packages/*'  # Shared packages
```

## Folder Structure for Monorepo

```
my-monorepo/
├── apps/
│   ├── web/              # Main React app
│   │   └── package.json
│   └── admin/            # Admin React app
│       └── package.json
├── packages/
│   ├── ui/               # Shared component library
│   │   ├── src/
│   │   └── package.json
│   ├── utils/            # Shared utilities
│   │   └── package.json
│   └── types/            # Shared TypeScript types
│       └── package.json
├── pnpm-workspace.yaml
└── package.json
```

## Using Shared Packages

```json
// apps/web/package.json
{
  "dependencies": {
    "@mycompany/ui": "workspace:*",
    "@mycompany/utils": "workspace:*",
    "@mycompany/types": "workspace:*"
  }
}
```

```typescript
// apps/web/src/components/UserCard.tsx
import { Button, Avatar } from '@mycompany/ui'
import { formatDate } from '@mycompany/utils'
import type { User } from '@mycompany/types'
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section 23 — VS Code Setup

## What It Is

Standardized VS Code configuration that every developer on the team uses. Eliminates "works on my machine" issues with editor settings.

## .vscode/settings.json

```json
{
  // Format on save — auto-runs Prettier
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",

  // ESLint auto-fix on save
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": "explicit",
    "source.organizeImports": "never"
  },

  // TypeScript
  "typescript.tsdk": "node_modules/typescript/lib",
  "typescript.enablePromptUseWorkspaceTsdk": true,

  // Tailwind IntelliSense
  "tailwindCSS.experimental.classRegex": [
    ["cva\\(([^)]*)\\)", "[\"'`]([^\"'`]*).*?[\"'`]"],
    ["cx\\(([^)]*)\\)", "(?:'|\"|`)([^']*)(?:'|\"|`)"]
  ],

  // File exclusions from sidebar
  "files.exclude": {
    "node_modules": true,
    "dist": true,
    "coverage": true,
    ".turbo": true
  },

  // Search exclusions
  "search.exclude": {
    "node_modules": true,
    "dist": true,
    "pnpm-lock.yaml": true
  },

  // File associations
  "files.associations": {
    "*.css": "tailwindcss"
  }
}
```

## .vscode/extensions.json

```json
{
  "recommendations": [
    "esbenp.prettier-vscode",          // Prettier formatter
    "dbaeumer.vscode-eslint",          // ESLint
    "bradlc.vscode-tailwindcss",       // Tailwind IntelliSense
    "ms-vscode.vscode-typescript-next", // TypeScript
    "vitest.explorer",                 // Vitest test runner
    "ms-playwright.playwright",        // Playwright tests
    "github.copilot",                  // AI assistant
    "usernamehw.errorlens",           // Inline error display
    "christian-kohler.path-intellisense", // Path completion
    "mikestead.dotenv",               // .env file support
    "eamodio.gitlens"                 // Git blame inline
  ]
}
```

## .vscode/launch.json — Debug Configuration

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Debug Chrome",
      "type": "chrome",
      "request": "launch",
      "url": "http://localhost:3000",
      "webRoot": "${workspaceFolder}/src",
      "sourceMapPathOverrides": {
        "webpack:///src/*": "${webRoot}/*"
      }
    },
    {
      "name": "Debug Vitest",
      "type": "node",
      "request": "launch",
      "program": "${workspaceFolder}/node_modules/vitest/vitest.mjs",
      "args": ["run", "--reporter=verbose"],
      "smartStep": true,
      "console": "integratedTerminal"
    }
  ]
}
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section 24 — Documentation Setup

## What It Is

Enterprise codebases need documentation — component documentation with Storybook, API documentation, and changelogs.

## Storybook Setup

```bash
npx storybook@latest init
```

## Component Story Example

```typescript
// src/components/ui/Button/Button.stories.tsx
import type { Meta, StoryObj } from '@storybook/react'
import { Button } from './Button'

const meta = {
  title: 'UI/Button',
  component: Button,
  parameters: {
    layout: 'centered',
  },
  tags: ['autodocs'],
  argTypes: {
    variant: {
      control: 'select',
      options: ['primary', 'secondary', 'destructive'],
    },
    size: {
      control: 'select',
      options: ['sm', 'md', 'lg'],
    },
  },
} satisfies Meta<typeof Button>

export default meta
type Story = StoryObj<typeof meta>

export const Primary: Story = {
  args: {
    variant: 'primary',
    children: 'Click me',
  },
}

export const Loading: Story = {
  args: {
    variant: 'primary',
    loading: true,
    children: 'Loading...',
  },
}

export const Disabled: Story = {
  args: {
    variant: 'primary',
    disabled: true,
    children: 'Disabled',
  },
}
```

## Automatic Changelog with Conventional Commits

```bash
pnpm add -D @changesets/cli
npx changeset init
```

When commits follow conventional commits format, changelogs can be generated automatically — every `feat:` becomes a new feature entry, every `fix:` becomes a bug fix entry.

[↑ Back to Table of Contents](#table-of-contents)

---

# Section 25 — Complete Setup Checklist

> Copy this and tick off each item when setting up a new enterprise React project.

## Foundation

```
  [ ] pnpm installed globally
  [ ] .nvmrc created with Node version
  [ ] Project created with Vite + React + TypeScript template
  [ ] package.json scripts complete (dev, build, lint, test, etc.)
  [ ] .gitignore complete (no .env files committed)
```

## TypeScript

```
  [ ] tsconfig.json with strict: true
  [ ] All strict flags enabled (noUnusedLocals, noImplicitReturns, etc.)
  [ ] Path aliases configured
  [ ] tsconfig.node.json created
```

## Build Tools

```
  [ ] vite.config.ts with manual chunks
  [ ] Bundle visualizer installed
  [ ] Path aliases match tsconfig
  [ ] API proxy configured for development
```

## Code Quality

```
  [ ] ESLint installed with TypeScript, React, accessibility plugins
  [ ] .eslintrc.cjs configured
  [ ] Prettier installed
  [ ] .prettierrc configured
  [ ] Stylelint installed (if using CSS/SCSS)
  [ ] .editorconfig created
```

## Git Workflow

```
  [ ] Husky installed (pnpm prepare runs install)
  [ ] pre-commit hook runs lint-staged
  [ ] commit-msg hook runs commitlint
  [ ] pre-push hook runs type-check and tests
  [ ] .lintstagedrc.cjs configured
  [ ] commitlint.config.cjs configured
```

## Environment

```
  [ ] .env.example committed to git
  [ ] .env.local in .gitignore
  [ ] Zod validation for environment variables in src/config/env.ts
  [ ] env.ts imported in main.tsx (validates at startup)
```

## Testing

```
  [ ] Vitest installed with jsdom environment
  [ ] React Testing Library installed
  [ ] @testing-library/jest-dom installed
  [ ] src/test/setup.ts created
  [ ] MSW installed for API mocking
  [ ] Playwright installed for E2E
  [ ] Coverage thresholds set (80% lines minimum)
```

## Project Structure

```
  [ ] Feature-based folder structure created
  [ ] Barrel files (index.ts) in each folder
  [ ] components/ui/ folder for shared UI components
  [ ] features/ folder for feature modules
  [ ] api/ folder with axios client configured
  [ ] store/ folder with Zustand setup
  [ ] config/ folder with env.ts
```

## State and API

```
  [ ] TanStack Query installed with QueryClientProvider in main.tsx
  [ ] ReactQueryDevtools added (development only)
  [ ] Axios client created with interceptors
  [ ] Zustand store created
  [ ] Query key factory pattern established
```

## Performance

```
  [ ] React Router with lazy loaded routes
  [ ] Manual chunks in vite build config
  [ ] Error boundaries added
```

## Security

```
  [ ] pnpm audit runs in CI
  [ ] No secrets in .env files committed
  [ ] No any types in production code
  [ ] Security headers added
```

## CI/CD

```
  [ ] .github/workflows/ci.yml created
  [ ] Quality job (TypeScript, ESLint, Prettier, audit)
  [ ] Test job with coverage upload
  [ ] Build job
  [ ] E2E job
```

## Docker (if needed)

```
  [ ] Multi-stage Dockerfile created
  [ ] nginx.conf configured for SPA routing
  [ ] .dockerignore created
```

## Developer Experience

```
  [ ] .vscode/settings.json committed
  [ ] .vscode/extensions.json committed
  [ ] .vscode/launch.json for debugging
  [ ] Storybook installed (for component library projects)
```

---

## Quick Reference — What Each Tool Does

| Tool | Purpose | When It Runs |
|---|---|---|
| TypeScript | Type checking | On save (editor), pre-push (husky), CI |
| ESLint | Code quality rules | On save (editor), pre-commit (husky), CI |
| Prettier | Code formatting | On save (editor), pre-commit (husky), CI |
| Stylelint | CSS quality rules | On save (editor), pre-commit (husky) |
| Husky | Run scripts on git events | On commit, on push |
| lint-staged | Run linters on staged files only | On commit |
| Commitlint | Validate commit messages | On commit |
| Vitest | Unit and component tests | On demand, pre-push, CI |
| Playwright | End-to-end browser tests | On demand, CI |
| MSW | Mock API in tests | During testing |
| TanStack Query | Server state management | Runtime |
| Zustand | Client state management | Runtime |
| Axios | HTTP client | Runtime |
| Vite | Dev server and build tool | Development and build |
| Docker | Container packaging | Build and deployment |
| GitHub Actions | Automated CI/CD | On push and PR |

[↑ Back to Table of Contents](#table-of-contents)