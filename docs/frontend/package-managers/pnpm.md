# PNPM

This document provides a **complete guide** for using **PNPM** in a monorepo setup with Expo React Native and
TypeScript.

---

## What is PNPM?

PNPM is a **fast, disk-efficient package manager** for Node.js projects. It is designed to handle monorepos efficiently
by using a **content-addressable storage system**, avoiding duplicated node_modules, and supporting **workspaces**
natively.

### Benefits

- **Monorepo-friendly**: Manages multiple packages in one repository efficiently.
- **Disk-space efficient**: Stores each package only once on your disk.
- **Fast installation**: Uses caching and symlinks to speed up dependency installs.
- **Strict dependency resolution**: Ensures consistent dependency trees across workspaces.
- **Workspace support**: Enables running scripts and commands across packages easily.

---

## Installing PNPM

### Global Installation

```bash
npm install -g pnpm
```

Check the version:

```bash
pnpm -v
```

### Project Setup

Initialize a new monorepo:

```bash
mkdir my-monorepo
cd my-monorepo
pnpm init
```

---

## Monorepo Structure

A typical monorepo structure with PNPM looks like this:

```
my-monorepo/
    apps/
        mobile/       # Expo React Native app
        backend/      # Optional backend/API
    packages/
        ui/           # Shared UI components
        utils/        # Shared TypeScript utilities
        config/       # Shared ESLint/TS/Prettier configs
    pnpm-workspace.yaml
    package.json
```

### `pnpm-workspace.yaml`

```yaml
packages:
  - "apps/*"
  - "packages/*"
```

This file tells PNPM which directories are **workspaces**. Each workspace can depend on others, and PNPM will link them
automatically.

---

## Root package.json

```json
{
  "name": "my-monorepo",
  "private": true,
  "devDependencies": {
    "typescript": "^5.3.0",
    "eslint": "^8.50.0"
  },
  "scripts": {
    "dev": "pnpm -r run dev",
    "build": "pnpm -r run build",
    "lint": "pnpm -r run lint",
    "typecheck": "pnpm -r run typecheck",
    "clean": "pnpm -r run clean"
  }
}
```

**Notes:**

- `-r` (or `--recursive`) tells PNPM to **run scripts in all workspaces**.
- `private: true` ensures this repository is not accidentally published.

---

## Example Workspace: Mobile App (apps/mobile/package.json)

```json
{
  "name": "mobile",
  "private": true,
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

- `dev` starts the Expo development server.
- `build` exports the app for production.
- `lint` and `typecheck` enforce code quality.

---

## Example Workspace: Shared UI Package (packages/ui/package.json)

```json
{
  "name": "ui",
  "version": "1.0.0",
  "main": "dist/index.js",
  "types": "dist/index.d.ts",
  "scripts": {
    "build": "tsc --build",
    "lint": "eslint src/",
    "typecheck": "tsc --noEmit"
  },
  "peerDependencies": {
    "react": "^18.2.0",
    "react-native": "*"
  }
}
```

- PNPM will **link this package to dependent workspaces automatically**.
- Use `pnpm install` after changing dependencies to ensure links are updated.

---

## Running Scripts Across Workspaces

### Start dev servers for all apps

```bash
pnpm -r run dev
```

### Build all packages and apps

```bash
pnpm -r run build
```

### Lint all workspaces

```bash
pnpm -r run lint
```

### Typecheck everything

```bash
pnpm -r run typecheck
```

---

## Targeting a Specific Workspace

### Run dev only for mobile

```bash
pnpm --filter mobile run dev
```

### Run lint only for ui package

```bash
pnpm --filter ui run lint
```

PNPM supports **workspace filtering**, allowing you to focus on a specific app or package.

---

## Managing Dependencies

- Install dependency for a specific workspace:

```bash
pnpm add react-native --workspace mobile
```

- Install dev dependency for all workspaces:

```bash
pnpm add -D typescript -r
```

- Remove a dependency:

```bash
pnpm remove <package> --workspace <workspace-name>
```

---

## Best Practices

- Keep **apps/** minimal; push reusable code into **packages/**.
- Use **workspace linking** instead of relative paths for shared packages.
- Centralize configs (`eslint`, `tsconfig`, `prettier`) in **packages/config/**.
- Use `peerDependencies` for shared React/React Native versions.
- Use `pnpm filter` for targeted builds and faster iteration.

---

## Troubleshooting & Tips

- **Node_modules duplication**: PNPM avoids duplication automatically; check `pnpm-lock.yaml` if conflicts arise.
- **Symlink issues**: Ensure your IDE supports PNPM symlinks for TypeScript resolution.
- **Expo Metro issues**: Use `expo start -c` to clear the cache.
- **Dependency mismatch**: Always use `pnpm -r install` after updating workspace dependencies.

---

## Summary

PNPM enables **fast, consistent monorepo management** for Expo React Native and TypeScript:

- Efficient workspace linking and dependency resolution.
- Recursive scripts across apps and packages.
- Supports filtering and targeted workspace commands.
- Saves disk space while ensuring strict dependency consistency.

---

## References

- [PNPM Official Docs](https://pnpm.io/)
- [PNPM Workspaces](https://pnpm.io/workspaces)
