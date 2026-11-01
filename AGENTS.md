# AGENTS.md — freellmapi

Local-first aggregator for free LLM API tiers. OpenAI-compatible proxy on `:3001`, React dashboard on `:5173`.
npm workspaces monorepo: `server/` (Express + SQLite, vitest), `client/` (Vite + React), `cli/`, `shared/types.ts`. `desktop/` is separate (Electron, own npm prefix).

## Commands (Node 22, see `.nvmrc`)
- setup: `./scripts/dev-bootstrap.sh` (installs deps, creates `.env`)
- dev: `npm run dev` · migrations: `npm run db:migration:up`
- verify before saying done: `npm test && npm run lint && npm run build`
- schema touched: also `npm run test:migrations`
- i18n touched: `cd client && npm run check:i18n`

## Where things live
- providers: `server/src/providers/<name>.ts`, extend `BaseProvider` in `base.ts`, `register(...)` in `index.ts`
- HTTP routes: `server/src/routes/` · logic: `server/src/services/`
- migrations: `server/src/db/migrations/`, one file per change, created with `npm run db:migration:create --name=...`
- architecture: `docs/en/architecture/00-high-level-index.md`

## Rules
- Plan first for anything over ~3 files; show the plan, wait for my ok.
- One change per branch/PR. Every change ships a test; keep the suite green.
- Never edit an applied migration. Add a new one.
- Only genuinely free, no-card providers. No paid or card-gated services.
- `client/src/i18n/locales/en.json` is the source of truth; mirror keys, don't hand-translate silently.
- Never commit `.env` or real API keys; `.env.example` gets placeholders only.
- Don't touch `desktop/`, release workflows, or `package-lock.json` unless the task is about them.
- After each edit, run the narrowest relevant test, then the full verify line before calling it done. Report PASS / FAIL / NOT RUN per command.

## Not for
- Routing Structure Therapeutics or client confidential data through free tiers. Treat every provider as public.
