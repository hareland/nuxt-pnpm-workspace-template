# nuxt-pnpm-workspace-template

> Parts of this file/workspace is copied
> from [nuxt-pnpm-workspace-template](https://github.com/hareland/nuxt-pnpm-workspace-template) and should be updated as
> you develop this app.

A simple scaffolding for building nuxt apps with pnpm workspace.

## Apps

- [@workspace/web](./apps/web) - The main web application (product).

> This should always be an up-to-date list.

> Setup a new app by running this from the workspace root:
>```bash
>cd apps
>```
>
>#### Nuxt:
>```bash
>pnpm create nuxt@latest -t gh:hareland/nuxt-app-template <app-name>
>```
>
>#### Any repo:
>```bash
>pnpx giget@latest gh:hareland/nuxt-app-template <app-name>
>```
>

## Packages
- [@workspace/eslint-config](./packages/eslint-config)
>This should always be an up-to-date list.



# Building on this template

## Clone & Create from this template:

```bash
pnpx giget@latest gh:hareland/nuxt-pnpm-workspace-template <app-name>
```

> You can add arguments such as `--install` to automatically run "pnpm install" after cloning the template.

## Add an existing app to the workspace

> Check out how [giget](https://github.com/unjs/giget) works.

```bash
pnpx giget@latest <source:gh/ssh++> <destination>
```