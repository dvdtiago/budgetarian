# Budgetarian Changes

Running log of every divergence from upstream Actual Budget. Keep this updated as features land — it makes merging upstream fixes much easier.

---

## Phase 0 — Fork setup (2026-05-01)

- Added `CLAUDE.md` budgetarian context block (appended after upstream `@AGENTS.md` reference)
- Added `BUDGETARIAN_CHANGES.md` (this file)
- Added `deploy/docker-compose.yml` — production single-container deployment on port 731
- `sync-server.Dockerfile`: bypassed lage (fails to discover workspaces in Docker) with explicit per-workspace build commands
- `sync-server.Dockerfile`: fixed runtime crash — replaced dangling `@actual-app/crdt` workspace symlink with copied `dist/` artifacts (same pattern already used for `@actual-app/web`)

---

## Planned changes (not yet implemented)

### Phase 1 — Data model
- New SQLite migration: `debt` and `debt_payment` and `balance_adjustment` tables (loot-core)
- New SQLite migration: `goal` and `goal_contribution` tables (loot-core)
- New SQLite migration: `surplus_allocation` table (loot-core)
- New TypeScript types: `packages/loot-core/src/types/models/debt.ts`
- New TypeScript types: `packages/loot-core/src/types/models/goal.ts`
- New server modules: `packages/loot-core/src/server/debt/`
- New server modules: `packages/loot-core/src/server/goals/`
- New server modules: `packages/loot-core/src/server/surplus/`

### Phase 2 — Debt UI
- New pages: `packages/desktop-client/src/components/debt/`
- Router entry: `/debt` route in App.tsx

### Phase 3 — Goals UI
- New pages: `packages/desktop-client/src/components/goals/`
- Router entry: `/goals` route in App.tsx

### Phase 4 — Surplus modal
- New component: `packages/desktop-client/src/components/surplus/SurplusModal.tsx`
- Budget page modification: add Debt Payments section + Goals section + Unbudgeted button

### Phase 5 — PHP/USD Income
- New income model additions to loot-core (originalAmount, exchangeRate, originalCurrency fields)
- New income page: `packages/desktop-client/src/components/income/`

### Phase 6 — Dashboard
- New page: `packages/desktop-client/src/components/dashboard/`
- Router entry: `/` (home) redirects to dashboard

### Phase 7 — Trends
- New page: `packages/desktop-client/src/components/trends/`
- Debt-specific charts: total debt over time, balance by debt, actual vs projected

### Phase 8 — Email reminders
- New cron job in `packages/sync-server/src/app-budgetarian/reminders.ts`
- New API routes: `packages/sync-server/src/app-budgetarian/`

### Phase 8 — Production Docker image
- Verify `sync-server.Dockerfile` builds cleanly with our additions
- CI/CD: push image to Docker Hub on merge to main
