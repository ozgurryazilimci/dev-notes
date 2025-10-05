# Turborepo

This document provides a **comprehensive guide** on using **Turborepo** in a monorepo setup with Expo React Native and
TypeScript, including how things work and why they are structured that way.

---

## What is Turborepo?

Turborepo is a **high-performance build system** and **task orchestrator** for JavaScript/TypeScript monorepos. It
manages **tasks across multiple packages and apps**, ensuring efficient execution, caching, and parallelization.

### Benefits

- **Monorepo support**: Keep multiple apps (mobile, backend) and shared packages in a single repository for reusable
  code and consistent tooling.
- **Task orchestration**: Automatically run tasks like dev, build, lint, or typecheck in the correct order.
- **Caching**: Skip unchanged tasks for faster builds.
- **Remote caching**: Share cache across machines or CI pipelines to reduce redundant work.
- **Parallel execution**: Run independent tasks simultaneously, speeding up the workflow.

---

## Typical Monorepo Structure

```
my-monorepo/
  .turbo/         # Internal cache for Turborepo (auto-generated)
  apps/
    mobile/       # Expo React Native app
    backend/      # API service (optional)
  packages/
    ui/           # Shared React Native components
    utils/        # Shared TypeScript utilities
    config/       # Shared ESLint/TS/Prettier configs
  turbo.json      # Turborepo pipeline definition
  package.json    # Root workspace + scripts
```

**How this works:**

- **apps/**: Each app is self-contained and may depend on shared packages.
- **packages/**: Holds reusable modules. Changes here may trigger builds in apps that depend on them.
- **.turbo/**: Cache directory storing task results for faster subsequent runs.
- **turbo.json**: Defines pipelines, dependencies, and outputs for tasks.
- **Root package.json**: Defines workspaces and root scripts to orchestrate tasks.

### The monorepo problem

Monorepos have many advantages - but they struggle to scale. Each workspace has its own test suite, its own linting, and
its own build process. A single monorepo might have thousands of tasks to execute.

![](<images/why-turborepo-problem.avif>)

These slowdowns can dramatically affect the way your teams build software, especially at scale. Feedback loops need to
be fast so developers can deliver high-quality code quickly.

### The monorepo solution

![](<images/why-turborepo-solution.avif>)

Turborepo solves your monorepo's scaling problem. Remote Cache stores the result of all your tasks, meaning that your CI
never needs to do the same work twice.

Additionally, task scheduling can be difficult in a monorepo. You may need to build, then test, then lint...

Turborepo schedules your tasks for maximum speed, parallelizing work across all available cores.

Turborepo can be adopted incrementally, and you can add it to any repository in just a few minutes. It uses the
`package.json` scripts you've already written, the dependencies you've already declared, and a single `turbo.json` file.
You can use it with any package manager, like `npm`, `yarn` or `pnpm` since Turborepo leans on the conventions of
the npm ecosystem.

---

## Installation

Get started by creating a new Turborepo:

```bash
npx create-turbo@latest
```

Or add to an existing monorepo:

```bash
npm install --save-dev turbo
```

Global install (optional):

```bash
npm install turbo --global
```

> Then, you can use the `turbo` command in your terminal.

---

## Root package.json

```json
{
  "name": "my-monorepo",
  "private": true,
  "workspaces": [
    "apps/*",
    "packages/*"
  ],
  "scripts": {
    "dev": "turbo run dev",
    "build": "turbo run build",
    "lint": "turbo run lint",
    "lint-fix": "turbo run lint:fix",
    "format": "turbo run format",
    "format-fix": "turbo run format:fix",
    "typecheck": "turbo run typecheck",
    "clean": "turbo run clean"
  },
  "devDependencies": {
    "turbo": "^1.12.0",
    "typescript": "^5.3.0"
  }
}
```

**Notes:**

- `"workspaces"`: Directs the package manager to link apps and packages automatically.
- `"scripts"`: Uses Turborepo to run each workspace’s respective script in the right order.
- `"devDependencies"`: Tools shared across all packages (Turborepo, TypeScript).

---

## turbo.json

```json
{
  "$schema": "https://turbo.build/schema.json",
  "pipeline": {
    "dev": {
      "cache": false,
      "persistent": true
    },
    "build": {
      "dependsOn": [
        "^build"
      ],
      "outputs": [
        "dist/**",
        "build/**"
      ]
    },
    "lint": {
      "outputs": []
    },
    "lint:fix": {
      "outputs": []
    },
    "format": {
      "outputs": []
    },
    "format:fix": {
      "outputs": []
    },
    "typecheck": {
      "dependsOn": [
        "^build"
      ],
      "outputs": []
    },
    "clean": {
      "cache": false
    }
  }
}
```

**How pipelines work:**

- Each **task** is a command like dev, build, lint, etc.
- `"dependsOn"`: Defines which tasks must run first. `"^build"` runs builds in dependent packages first.
- `"outputs"`: Files or directories produced by the task, used for caching.
- `"cache": false`: Tasks that shouldn’t be cached, like dev servers or clean.
- `"persistent": true`: Long-running tasks (like dev) are tracked without caching.

Turborepo builds a **task graph (DAG)** and runs tasks in parallel whenever dependencies allow.

---

## Example: Expo Mobile App (apps/mobile/package.json)

```json
{
  "name": "mobile",
  "scripts": {
    "dev": "expo start",
    "build": "expo export",
    "lint": "eslint . --ext .ts,.tsx",
    "typecheck": "tsc --noEmit"
  },
  "dependencies": {
    "expo": "^49.0.0",
    "react-native": "0.72.0"
  }
}
```

**Notes:**

- Each workspace defines its own scripts; Turborepo orchestrates their execution.
- `dev` starts the Expo dev server.
- `build` exports the app for production.
- `lint` and `typecheck` enforce code quality and type safety.

---

## Example: Shared UI Package (packages/ui/package.json)

```json
{
  "name": "ui",
  "main": "dist/index.js",
  "types": "dist/index.d.ts",
  "scripts": {
    "build": "tsc --build",
    "lint": "eslint src/",
    "typecheck": "tsc --noEmit"
  },
  "devDependencies": {
    "typescript": "^5.3.0"
  },
  "peerDependencies": {
    "react": "^18.2.0",
    "react-native": "*"
  }
}
```

**Notes:**

- `"peerDependencies"` prevent multiple React versions in the same project.
- `build` compiles TypeScript into `dist/`. Turbo uses `outputs` to cache builds.

---

## Running Tasks

```
# Start all apps (Expo + others)
npm run dev

# Build everything
npm run build

# Lint all workspaces
npm run lint

# Typecheck everything
npm run typecheck

# Run only mobile app build
turbo run build --filter=mobile
```

- Filtering allows you to **run tasks in specific packages**, avoiding unnecessary work.
- You can also filter lint, format, or dev tasks:

```
turbo run dev --filter=mobile
turbo run lint --filter=ui
```

This is useful when you only want to work on a **specific app or package** without affecting the whole monorepo.

---

## Pnpm Integration

If you use **pnpm** as your package manager, you can follow this document for additional setup and commands:
[PNPM](../../frontend/package-managers/pnpm.md)

---

## Advanced Features

- **Remote Caching**: Share task results across machines/CI to speed up builds.
- **Filtering**: Run tasks only for specific workspaces.
- **Parallel Execution**: Turbo automatically runs independent tasks at the same time.

---

## Best Practices

- Keep **apps/** lightweight; push reusable code into **packages/**.
- Centralize configurations in **packages/config/**.
- Use **consistent scripts** (`dev`, `build`, `lint`, `typecheck`, `clean`).
- Use **peerDependencies** for shared React/React Native packages.
- Enable **remote caching** for CI to save time.

---

## Troubleshooting & Common Issues

- **Metro cache issues (React Native)**: Clear with `expo start -c`.
- **TypeScript path issues**: Ensure `tsconfig.json` uses `baseUrl` and `paths` correctly.
- **Peer dependency warnings**: Check React/React Native/Expo versions match across packages.
- **Slow CI builds**: Enable remote caching in Turbo.

---

## Summary

Turborepo makes **Expo React Native monorepos** fast, scalable, and easy to maintain by:

- Orchestrating tasks across apps and packages.
- Using caching to skip unchanged tasks.
- Running tasks in parallel when possible.

**Steps to get started:**

1. Define workspaces in root `package.json`.
2. Configure pipelines in `turbo.json`.
3. Add standard scripts to each workspace.
4. Run tasks with `turbo run` and enjoy faster builds.

---

## References

- [Turborepo Official Docs](https://turborepo.com/docs)
