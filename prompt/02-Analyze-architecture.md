Analyze this repository and report on its architecture. Do NOT guess — actually inspect the relevant files before answering. Structure your answer exactly like this:

## 1. High-Level Architecture
- What framework(s) are in use (React, Next.js, Vue, Angular, etc.) — check package.json dependencies.
- Is this a single app, a monorepo (check for workspaces in package.json, nx.json, turbo.json, lerna.json, pnpm-workspace.yaml), or multiple independently deployable apps?
- List the top-level folder structure and briefly explain what each major folder does.
- Identify the build tool (Vite, Webpack, Next.js, CRA, Turbopack) by checking config files (vite.config.*, webpack.config.*, next.config.*).

## 2. Is This a Micro-Frontend or Just Page Routing?
Check for each of these specifically and report what you find:
- **Module Federation**: search for `ModuleFederationPlugin`, `@module-federation/*`, or `remotes`/`exposes`/`shared` keys in any webpack/vite config.
- **Next.js Multi-Zones**: check next.config.js for `basePath`, rewrites to other domains/ports, or multiple separate Next.js apps in the repo pointing at different path prefixes.
- **Iframe-based composition**: search the codebase for `<iframe` usage, especially in layout/shell components.
- **single-spa or Web Components composition**: search package.json for `single-spa`, `single-spa-react`, or custom element registration (`customElements.define`).
- **Plain routing**: if none of the above appear, and routing is handled by `react-router-dom`, `next/router`, or `next/navigation` with all routes rendered by ONE React tree, state clearly that this is just page routing within a single app, not a micro-frontend.
- Quote the specific file(s) and line(s) that support your conclusion.

## 3. Source Maps & React DevTools Configuration
- Check if source maps are enabled or disabled: look for `devtool` in webpack config, `sourcemap` in vite.config, `productionBrowserSourceMaps` in next.config.js, or `GENERATE_SOURCEMAP` in any .env file.
- Check if the app disables the React DevTools hook anywhere (search for `__REACT_DEVTOOLS_GLOBAL_HOOK__` or `isDisabled` in the codebase).
- Note whether this is a dev-only or also a production setting.

## 4. How to Debug This Specific Repo
Based on what you found above, give me concrete, repo-specific debugging steps:
- Exact commands to run this project locally in dev mode.
- Whether I should expect ONE React renderer or multiple (based on section 2 findings).
- Whether any part of this app runs server-side only (Next.js Server Components / API routes) and therefore won't appear in browser DevTools at all.
- Any project-specific gotchas (custom debug flags, required env vars, ports used for multiple apps if this is a multi-app/micro-frontend setup).

Be specific and cite actual file paths/line numbers from this repo — do not give generic advice that could apply to any React project.
