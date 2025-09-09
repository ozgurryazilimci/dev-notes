# Yarn - JavaScript Package Manager

**Yarn** is an alternative package manager for JavaScript that works with Node.js.  
It is designed to be faster, more reliable, and more secure than **npm**, while still being compatible with the same
package ecosystem.

---

## Installation

On macOS with Homebrew:

    brew install yarn

Check version:

    yarn -v

---

## Basic Commands

Initialize a new project:

    yarn init -y

Install dependencies from `package.json`:

    yarn install

Add a new dependency:

    yarn add react

Add a development dependency:

    yarn add --dev webpack

Remove a dependency:

    yarn remove react

Upgrade a dependency:

    yarn upgrade react@latest

Run scripts from `package.json`:

    yarn start
    yarn build

---

## Global Packages

Add a global package:

    yarn global add typescript

Check global packages:

    yarn global list

Remove a global package:

    yarn global remove typescript

---

## Workspaces

Yarn supports **monorepos** with workspaces.

Add to root `package.json`:

    {
      "private": true,
      "workspaces": [
        "packages/*"
      ]
    }

Install dependencies across all workspaces:

    yarn install

---

## Comparison with npm

| Feature              | npm                          | yarn                              |
|----------------------|------------------------------|-----------------------------------|
| Default with Node.js | ✅ Yes                        | ❌ No (needs install)              |
| Speed                | Slower (sequential installs) | Faster (parallel installs, cache) |
| Lockfile             | `package-lock.json`          | `yarn.lock`                       |
| Workspaces           | Supported (npm v7+)          | Native support, very stable       |
| CLI command prefix   | `npm run script`             | `yarn script` (shorter syntax)    |

---

## Integration with nvm

If you switch Node.js versions with **nvm**, you may need to reinstall global Yarn:

    nvm install node --reinstall-packages-from=$(nvm current)

---

## Useful Commands

Clear cache:

    yarn cache clean

Check outdated packages:

    yarn outdated

Audit for vulnerabilities:

    yarn audit

---

## References

- [Yarn Official Website](https://yarnpkg.com/)
- [Yarn GitHub](https://github.com/yarnpkg/yarn)
- [npm vs Yarn](https://classic.yarnpkg.com/en/docs/migrating-from-npm/)  
