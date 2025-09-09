# npm ↔ Yarn Migration Cheatsheet

This cheatsheet shows the most common **npm** commands and their **Yarn** equivalents.  
Use it when migrating projects or switching between the two package managers.

---

## Install Dependencies

    npm install
    yarn install

---

## Add Dependencies

Install a package:

    npm install react
    yarn add react

Install as a dev dependency:

    npm install --save-dev webpack
    yarn add --dev webpack

Install globally:

    npm install -g typescript
    yarn global add typescript

---

## Remove Dependencies

    npm uninstall react
    yarn remove react

---

## Update Dependencies

Update a single package:

    npm update react
    yarn upgrade react

Update to latest version:

    npm install react@latest
    yarn upgrade react@latest

---

## Run Scripts

    npm run start
    yarn start

    npm run build
    yarn build

---

## Check Versions

    npm -v
    yarn -v

    node -v   (same for both)

---

## Cache

    npm cache clean --force
    yarn cache clean

---

## Audit & Outdated

Check for outdated packages:

    npm outdated
    yarn outdated

Audit for vulnerabilities:

    npm audit
    yarn audit

---

## References

- [npm Docs](https://docs.npmjs.com/)
- [Yarn Docs](https://yarnpkg.com/getting-started/migration)  
