# nuxt-pnpm-workspace-template

A simple scaffolding for building Nuxt apps with a pnpm workspace.

## Quick Start

### Create a new workspace from this template

```bash
pnpx giget@latest gh:hareland/nuxt-pnpm-workspace-template <app-name>
```

> Add `--install` to automatically run `pnpm install` after cloning.

### Add a new app to the workspace

From the workspace root:

```bash
cd apps
```

**Nuxt app:**

```bash
pnpm create nuxt@latest -t gh:hareland/nuxt-app-template <app-name>
```

**Any other repo/template:**

```bash
pnpx giget@latest gh:hareland/nuxt-app-template <app-name>
```

### Add an existing app to the workspace

Uses [giget](https://github.com/unjs/giget):

```bash
pnpx giget@latest <source:gh/ssh++> <destination>
```

## Apps

- [@workspace/web](./apps/web) — the main web application (product)

> Keep this list up to date.

## Packages

- [@workspace/eslint-config](./packages/eslint-config)

> Keep this list up to date.

---

> Parts of this file/workspace were copied
> from [nuxt-pnpm-workspace-template](https://github.com/hareland/nuxt-pnpm-workspace-template) and should be updated as
> you develop this app.