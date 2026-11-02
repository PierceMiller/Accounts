# Accounts — Personal Finance Dashboard

A private New Zealand-dollar dashboard using Supabase Auth, Postgres row-level security, and an Akahu Edge Function.

## Backend
- Supabase project: `qowxsqejqszyrabivgxn`
- Edge Function: `akahu-sync` (JWT verification enabled)
- Frontend uses a Supabase publishable key only.
- Never put Akahu tokens or Supabase service-role keys in this repository.

## One-time setup
1. Deploy this repository using GitHub Pages (Settings → Pages → Deploy from branch → `master` → `/ (root)`).
2. In Supabase Authentication → URL Configuration, set the site URL to the GitHub Pages URL and add that URL to Redirect URLs.
3. Create your personal account through the dashboard. If email confirmation is enabled, confirm the email and sign in.
4. In the dashboard's Setup status section, copy your user ID.
5. In Supabase → Edge Functions → Secrets, confirm `AKAHU_APP_TOKEN` and `AKAHU_USER_TOKEN` are present. Add `FINANCE_ALLOWED_USER_ID` with your user ID. The sync function is intended to reject every other authenticated user.
6. Test the Sync Kiwibank button.

The finance tables use RLS. If the dashboard says the tables cannot be loaded, check Supabase Data API exposure settings and ensure authenticated grants remain in place. Keep RLS enabled.
