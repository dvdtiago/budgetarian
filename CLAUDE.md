@AGENTS.md
@.github/agents/pr-and-commit-rules.md

---

# Budgetarian fork — additional context

This is a personal fork of Actual Budget extended with debt payoff tracking, savings goals, PHP/USD income logging, an avalanche surplus allocator, and a dashboard. Built for David's debt-payoff goal.

## Upstream sync

- Upstream remote: `https://github.com/actualbudget/actual.git`
- Pull upstream fixes: `git fetch upstream && git merge upstream/master`

## Our additions

| Location | What |
|---|---|
| `packages/loot-core/migrations/` | New SQL migration files for debt/goals/surplus tables |
| `packages/loot-core/src/types/models/debt.ts` | Debt TypeScript types |
| `packages/loot-core/src/types/models/goal.ts` | Goal TypeScript types |
| `packages/loot-core/src/server/debt/` | Debt CRUD, amortization, avalanche logic |
| `packages/loot-core/src/server/goals/` | Goals CRUD + contributions |
| `packages/loot-core/src/server/surplus/` | Monthly surplus calculation + plan |
| `packages/desktop-client/src/components/debt/` | Debts UI page |
| `packages/desktop-client/src/components/goals/` | Goals UI page |
| `packages/desktop-client/src/components/surplus/SurplusModal.tsx` | Unbudgeted allocator modal |
| `packages/desktop-client/src/components/dashboard/` | Dashboard page |
| `packages/desktop-client/src/components/income/` | Income page (PHP/USD) |
| `packages/sync-server/src/app-budgetarian/` | Server-side routes + email reminder cron |
| `deploy/docker-compose.yml` | Production single-container deploy |
| `BUDGETARIAN_CHANGES.md` | Running log of every divergence from upstream |

## Domain rules

- Primary currency: **PHP (₱)**. USD income stored as `originalAmount + exchangeRate + amountPhp`
- Surplus = totalIncome − totalExpenses − totalDebtPaid (computed per month)
- Avalanche: debts sorted by interest rate descending; auto-split caps at currentBalance
- Debt statuses: `ACTIVE | PLANNED | PAID_OFF`
- Styling: use Emotion theme tokens (upstream pattern) — not Tailwind

## Production deploy

```bash
# Build and push image
docker build -f sync-server.Dockerfile -t dvdtiago/budgetarian:latest .
docker push dvdtiago/budgetarian:latest

# On server (dvdserver@dvdserver-ubuntu, ~/Apps/budgetarian)
docker compose pull && docker compose up -d
```

## Commit messages in this repo

Upstream requires `[AI]` prefix on all AI commits (see AGENTS.md). Follow this.
