# Gamification test branch — implementation plan

## Repository inspection (2026-11-20)

- Repository: `PierceMiller/Accounts` (public; push permission available).
- Production/default branch: `master`.
- Current application: single-page `index.html` (about 81 KB), vanilla HTML/CSS/JavaScript ES module, Supabase JS v2.49.1.
- Existing finance data is read from `finance_accounts`, `finance_transactions`, `finance_budgets`, `finance_recurring_items`, and `finance_debts`.
- Existing UI tabs: Dashboard, Transactions, AI Budget, Bills & debts, Report archive.
- Existing GitHub Pages workflow deploys **only pushes to master**. A push to this feature branch will not deploy the production site.
- Production uses Supabase project `qowxsqejqszyrabivgxn`; this branch must not add writes to any finance table or change its schema.

## User-selected direction

- RPG progression.
- Balanced challenge intensity.
- Both real-world financial progress and virtual rewards.

## Safety architecture

1. This branch is isolated from `master`; no production workflow or files are changed.
2. First preview uses synthetic example values only and has no Supabase import, network requests, credentials, or access to real financial data.
3. The eventual integration will consume a read-only, minimal finance snapshot (monthly cash-flow status, budget adherence, debt trend and confirmed savings milestones).
4. XP/quest/achievement state must be stored separately from finance records. Prefer a dedicated staging Supabase project/table after explicit approval; local-only prototype state is acceptable for UX validation.
5. Never store transaction descriptions, merchant names, account identifiers, balances, or sensitive financial details in achievement events.
6. XP awards are deterministic and idempotent: stable event keys, unique constraints in a future isolated progression store, and no repeat award on refresh/re-sync.
7. Refunds, reversals, pending items, internal transfers and P W MILLER transactions must not generate positive progress.
8. No challenge should encourage skipping essentials, borrowing, overspending, or making unnecessary purchases. Streaks should allow recovery and never punish hardship.

## Delivery phases

### Phase 1 — UX prototype (this commit)
- Create a standalone preview page with fictional demo values.
- RPG profile, XP bar, levels, quests, achievements, boss progress, and virtual unlocks.
- Clearly label all values as demo data; no backend connection.

### Phase 2 — read-only finance adapter
- Add an adapter in the app that derives only safe aggregate signals from the already-loaded finance data.
- Use the same settled transaction and transfer-exclusion rules as the dashboard.
- Never expose raw transactions to the gamification UI.

### Phase 3 — progression persistence
- Decide between local-only progress and a separate staging Supabase project.
- If Supabase is chosen, create progression-only tables with RLS and unique event keys; do not use finance tables.
- No production schema changes without explicit approval.

### Phase 4 — testing and review
- Verify no writes to finance tables, no new secrets, no Akahu sync changes, no duplicate XP, and correct NZ/Pacific-Auckland month boundaries.
- Test mobile layout, signed-out state, empty data, stale sync, reversals and missing history.
- Keep this branch preview-only until the user explicitly approves a merge/deploy.

## Acceptance criteria
- The live `master` branch and current GitHub Pages deployment remain unchanged.
- The preview works offline with synthetic data.
- Every reward has a clear explanation and can be audited.
- Financial progress is never inferred from incomplete/stale bank data.
- No production financial record is created, updated, or deleted.
