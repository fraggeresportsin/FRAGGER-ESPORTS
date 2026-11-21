# FRAGGER ESPORTS — website starter

React + Vite frontend, Supabase Auth and PostgreSQL schema.

## Chosen configuration
- Domain target: `fraggeresports.in`
- Multi-game: BGMI, Free Fire, Valorant, Call of Duty Mobile, Other
- Registration: players can create accounts; captains submit team registrations
- Payments: manual UPI/reference verification by management
- Modules: tournament discovery, team registration, captain's entries, leaderboard, admin approval and score entry

## Run locally
Requires Node.js 20+.
```bash
npm install
cp .env.example .env
npm run dev
```
Without Supabase, the site displays demo tournaments only. Demo registrations are not saved.

## Connect Supabase
1. Create a Supabase project.
2. Run `supabase/schema.sql` in its SQL Editor.
3. Copy Project URL and anon/public key into `.env` (`VITE_SUPABASE_URL`, `VITE_SUPABASE_ANON_KEY`).
4. Enable email authentication and set production Site URL / redirect URLs in Supabase Auth.
5. Create your account through the website's sign-up flow.
6. Promote your own account:
```sql
update public.profiles set role='admin'
where id=(select id from auth.users where email='YOUR_EMAIL');
```
Never place the Supabase service-role key in frontend code.

## Deploy to Vercel
1. Push this folder to a private GitHub repository.
2. Import it in Vercel; select Vite, build command `npm run build`, output directory `dist`.
3. Add the Supabase environment variables in Vercel.
4. Deploy, then add `fraggeresports.in` under Project → Settings → Domains.
5. At your domain registrar, add the DNS records Vercel shows. Use the exact current values from Vercel.
6. Add `https://fraggeresports.in` and your preview URL to Supabase Auth URL configuration.

## Before accepting real registrations or money
This is a working starter, not a security-audited production tournament service. Complete:
- Atomic slot reservation via a Postgres RPC/transaction (avoid overselling the final slot).
- Email/phone verification, CAPTCHA, rate limits and admin MFA.
- Admin UI for payment reference verification, POV upload review, prize payout, disqualification and announcements.
- Private POV storage with signed URLs, retention policy and access logs.
- Audit log for admin decisions and score changes.
- Test RLS as anonymous, player, captain and admin; run Supabase security advisor.
- Publish privacy policy, terms, tournament rules, refund/payment policy and contact/dispute details.
- Check each game's publisher rules for community tournaments and paid entry.

Demo tournament cards are sample data only, not actual open events or verified standings.
