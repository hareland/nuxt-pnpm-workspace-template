# AGENTS.md

Guidance for AI coding agents (and humans) working in this repository.

## Repo layout

```
/apps/web         Nuxt 4.5 application (the product)
/apps/**          Other Apps - could be any language or deployment target.
/packages/**      Shared workspace packages (ui, config, types, utils, etc.)
```

This is a pnpm workspace monorepo. Packages under `packages/**` are consumed
by `apps/web` (and each other) via workspace protocol dependencies
(`"@scope/name": "workspace:*"`). Assume pnpm unless a package's own README
says otherwise — adjust commands accordingly if this repo actually uses
npm/yarn.

All commands below are run from the repo root. Never run `npm install` /
`yarn` inside a subfolder — it will desync the lockfile.

---

## Setup

```bash
pnpm install          # installs all workspaces from repo root; do not install per-package
```

---

## Common commands

```bash
pnpm dev                             # start apps/web dev server
pnpm --filter ./apps/web dev         # same, explicit filter
pnpm build                           # build all workspaces
pnpm --filter ./apps/web build       # build only the web app
pnpm --filter '<pkg-name>' build     # build a single package (name = "name" field in its package.json)
pnpm typecheck                       # nuxi typecheck / tsc across workspaces
pnpm lint                            # eslint across workspaces
pnpm lint:fix
pnpm test                            # run all tests
pnpm --filter '<pkg-name>' test      # run tests for one package
```

If `turbo.json` or `nx.json` exists at the root, prefer
`pnpm turbo run <task>` / `pnpm nx run-many -t <task>` over looping `--filter`
manually — check before assuming plain pnpm scripts are the only path.

---

## `apps/web` (Nuxt 4.5)

- Uses the Nuxt 4 directory structure: source lives under `apps/web/app/`
  (`app/components`, `app/composables`, `app/pages`, `app/layouts`,
  `app/middleware`, `app/utils`). Server code (event handlers, server-only
  utils) lives in `apps/web/server/`. Don't recreate the old Nuxt 3
  top-level `components/`, `pages/`, etc. folders outside `app/`.
- `nuxt.config.ts` is the source of truth for modules, runtime config, and
  aliases — check it before assuming a module or alias exists.
- Auto-imports are enabled for `app/components`, `app/composables`, and
  `app/utils`. Don't manually import things Nuxt already auto-imports; do
  add explicit imports for anything pulled from a `packages/**` dependency.
- Prefer `<script setup lang="ts">` for all new components.
- Use Nuxt's `runtimeConfig` (`nuxt.config.ts`) for environment variables,
  not raw `process.env` reads inside components/composables.
- After changing `nuxt.config.ts`, module options, or adding a new
  `packages/**` dependency, restart the dev server / rerun `nuxi prepare` so
  `.nuxt/` types regenerate — stale `.nuxt` types are a common false-positive
  source for typecheck errors.

## `packages/**` (shared workspace packages)

- Each package is independently publishable/consumable: it must have its
  own `package.json` with a `name`, `main`/`exports`, and `types` field.
  Don't add code to a package without checking its `exports` map —
  unexported files aren't reachable from `apps/web`.
- Keep packages framework-agnostic where possible. Nuxt-specific glue (Vue
  composables that need Nuxt context, Nuxt modules) belongs in a clearly
  named package (e.g. `packages/nuxt-*`) or directly in `apps/web`, not
  mixed into generic utility packages.
- When you change a package's public API, check for consumers with:
  ```bash
  pnpm -r exec grep -l "@scope/<pkg-name>" package.json
  ```
  and update `apps/web` (and any other consuming package) in the same change.
- Build packages before relying on their compiled output in `apps/web` if
  the workspace resolves compiled (not source) output — check whether
  `apps/web`'s Vite/Nuxt config aliases packages to `src` or to `dist`.
- New packages: match the existing structure of a sibling package
  (tsconfig, build tool, test setup) rather than introducing a new toolchain.

---

## Code style

- TypeScript everywhere; avoid `any` — use `unknown` + narrowing, or
  generics.
- Follow the repo's ESLint/Prettier config as configured at the root
  (`eslint.config.*` / `.prettierrc*`); don't hand-roll formatting that
  conflicts with it. Run `pnpm lint --fix` before finishing a change.
- Match existing naming conventions per folder (e.g. composables prefixed
  `use*`, Pinia stores suffixed `Store`) rather than introducing new ones.

---

## Before committing / finishing a task

1. `pnpm lint`
2. `pnpm typecheck`
3. `pnpm test` (or scoped `--filter` if the change is isolated to one
   app/package)
4. `pnpm build` for anything touching `nuxt.config.ts`, package `exports`,
   or build tooling, to catch build-time-only errors

Don't skip the workspace-wide typecheck/build even for small changes — a
package's type or export change can silently break `apps/web`.

**Never commit generated or dependency files.** The following are always excluded via `.gitignore` and must never be staged or committed:
- `node_modules/` (root and any sub-package)
- `apps/web/.nuxt/` — Nuxt generated types and build cache
- `apps/web/.output/` — production build output
- `apps/web/.data/` — local NuxtHub/D1 database files
- `dist/` — compiled package output

If you run `pnpm install` or `nuxi prepare` as part of your work, make sure none of the above appear in `git status` before committing. Use `git rm -r --cached <path>` to untrack any files accidentally staged.

---

## PR / commit conventions

- Use Conventional Commits (`feat:`, `fix:`, `chore:`, `refactor:`, etc.).
- If the repo uses Changesets (`.changeset/` folder present), add a
  changeset for any change to a published `packages/**` package:
  ```bash
  pnpm changeset
  ```
  Not needed for `apps/web`-only changes.

---

## Notes for agents

- This file describes conventions inferred to be typical for a pnpm + Nuxt 4
  monorepo of this shape. If actual scripts in the root `package.json` or a
  package's own `README.md`/`AGENTS.md` differ from what's written here,
  those local sources of truth win — update this file to match reality when
  you notice a mismatch.
- Prefer adding shared logic to `packages/**` over duplicating it inside
  `apps/web`, but don't create a new package for a single one-off helper.