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

- [@workspace/web](./apps/web) - the main web application (product)

> Keep this list up to date.

## Packages

- [@workspace/eslint-config](./packages/eslint-config)

> Keep this list up to date.

## Updating an app via git subtree

Keeps an app folder (e.g. `apps/web`) in sync with its source template repository, without submodules.

> Variable names are just for convenience, swap with whatever fits your case.

### 1. Prepare variables

```bash
REMOTE_NAME=apps-web-template
REMOTE_REPOSITORY=git@github.com:hareland/nuxt-app-template.git
WORKSPACE_PATH=apps/web
```

> `REMOTE_NAME` is the name of the remote to use for fetching updates. <br/>
> `REMOTE_REPOSITORY` is the name of the template repository. <br/>
> `WORKSPACE_PATH` is the path to the app folder in the workspace. <br/>

### 2. Add the template repository as a remote

```bash
git remote add $REMOTE_NAME $REMOTE_REPOSITORY
git fetch $REMOTE_NAME
```

### 3. Add the subtree

Only needed once, when the app doesn't exist in the workspace yet:

```bash
git subtree add --prefix=$WORKSPACE_PATH $REMOTE_NAME main --squash
```

### 4. Pull future updates

> This step is the only one you need to repeat to update this in the future.

```bash
git fetch $REMOTE_NAME
git subtree pull --prefix=$WORKSPACE_PATH $REMOTE_NAME main --squash
```

> `--squash` keeps the workspace history clean by importing each sync as a single commit, rather than the template's
> full commit history.

---

> Parts of this file/workspace were copied
> from [nuxt-pnpm-workspace-template](https://github.com/hareland/nuxt-pnpm-workspace-template) and should be updated as
> you develop this app.