# Project Index: workspace

Generated: 2026-07-11

## Project Structure

```
.
├── apps/
│   ├── api/            Fastify backend app (@workspace/api)
│   └── api-e2e/         Jest e2e tests for api (@workspace/api-e2e)
├── .claude/             Claude Code config (settings, skills)
├── .agents/ .codex/ .opencode/ .omo/   Other agent-tool configs (Codex/OpenCode/etc.)
├── nx.json              Nx workspace config (plugins: @nx/vite, @nx/docker)
├── package.json         Root workspace manifest (bun workspaces: apps/*)
├── mise.toml            Task runner (install/serve/build/test/e2e wrapping `bunx nx`)
└── tsconfig.base.json    Shared TS config
```

## Entry Points

- API server: `apps/api/src/main.ts` — Fastify app bootstrap
- API app: `apps/api/src/app/app.ts` — Fastify plugin registration (autoload routes/plugins)
- API e2e: `apps/api-e2e/src/api/api.spec.ts` — smoke test hitting the running API

## Core Modules

### `@workspace/api` (`apps/api`)
- Path: `apps/api/src`
- Entry: `src/main.ts`
- Structure: `src/app/app.ts` (app builder), `src/app/plugins/sensible.ts` (fastify-sensible plugin),
  `src/app/routes/root.ts` (root route)
- Dependencies: `fastify`, `fastify-plugin`, `@fastify/autoload`, `@fastify/sensible`
- Nx targets: `build` (`@nx/esbuild:esbuild`), `serve` (`@nx/js:node`, depends on `build`),
  `test` (`@nx/jest:jest`), `prune-lockfile`, `copy-workspace-modules`, `prune`, `docker:build`

### `@workspace/api-e2e` (`apps/api-e2e`)
- Path: `apps/api-e2e/src`
- Purpose: end-to-end tests against `@workspace/api`
- Nx target: `e2e` (`@nx/jest:jest`, depends on `@workspace/api:build` + `:serve`)
- `implicitDependencies`: `@workspace/api`

## Configuration

- `nx.json` — Nx workspace config; inferred-task plugins `@nx/vite/plugin`, `@nx/docker`; Nx Cloud enabled (`nxCloudId`)
- `package.json` — root manifest, `packageManager: bun@1.3.14`, bun workspaces `apps/*`
- `mise.toml` — task runner wrapping `bunx nx` for install/serve/build/test/e2e
- `tsconfig.base.json` / `tsconfig.json` — shared TypeScript config
- `jest.config.ts` / `jest.preset.js` — root Jest preset
- `eslint.config.mjs` — ESLint flat config
- `.prettierrc` / `.prettierignore` — formatting
- `opencode.json` — OpenCode agent-tool config
- `.claude/settings.json` — Claude Code settings; declares `nx-claude-plugins` marketplace/plugin
- `.claude/skills/` — project-level Claude Code skills (nx-workspace, nx-generate, nx-import,
  nx-plugins, nx-run-tasks, monitor-ci, link-workspace-packages)

## Documentation

- `README.md` — Nx-generated workspace quickstart (run/build/generate/CI setup)
- `CLAUDE.md` / `AGENTS.md` — Nx-generated agent guidance (use `nx` CLI via package manager,
  invoke `nx-workspace`/`nx-generate` skills, check `nx_docs` for advanced config)

## Test Coverage

- Unit tests: 2 spec files (`apps/api/src/app/app.spec.ts`, plus co-located specs)
- E2E tests: 1 spec file (`apps/api-e2e/src/api/api.spec.ts`)
- Test runner: Jest via `@nx/jest:jest` executor (`nx test @workspace/api`, `nx e2e @workspace/api-e2e`)
- No coverage report configured/generated as of this indexing pass

## Key Dependencies

- `nx@23.0.2` — monorepo build/task orchestration (+ `@nx/vite`, `@nx/docker`, `@nx/jest`,
  `@nx/node`, `@nx/plugin`, `@nx/web`, `@nx/workspace`)
- `fastify@~5.2.1` — API HTTP framework (+ `fastify-plugin`, `@fastify/autoload`, `@fastify/sensible`)
- `jest@~30.3.0` — test runner
- `vite@^8.0.0` — build tooling (via `@nx/vite/plugin`)
- Package manager: `bun@1.3.14` (bun.lock is the lockfile of record; package-lock.json also present)

## Quick Start

1. `mise run install` (or `bun install`) — install dependencies
2. `mise run serve` (or `bunx nx serve @workspace/api`) — run the API dev server
3. `mise run test` (or `bunx nx test @workspace/api`) — run unit tests
4. `mise run e2e` (or `bunx nx e2e @workspace/api-e2e`) — run e2e tests
