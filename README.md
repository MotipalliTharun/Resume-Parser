# ATS Resume Suite

Enterprise-grade workflow application that transforms any baseline resume into an ATS-friendly package aligned with specific job descriptions. The suite guides you from ingestion to recruiter outreach, delivers downloadable bundles, and is ready for Vercel deployment with Supabase persistence.

## Highlights

- **Structured resume editing** – parse raw text into an editable schema and tailor sections for each job.
- **ATS scoring engine** – keyword coverage, semantic similarity, and actionable gap insights.
- **Recruiter email automation** – deterministic drafts with optional LLM enrichment.
- **Mobile-first UI** – responsive workflow canvas usable on phones and desktops.
- **Downloadable artifacts** – one-click ZIP bundle containing tailored resume, job, match metrics, and outreach draft.
- **Supabase integration** – authentication, persistence, and storage-ready schema helpers.

## Project Structure

```
src/
  app/
    page.tsx                # marketing landing page
    app/                    # authenticated workspace
    auth/                   # magic link sign-in
    api/                    # REST endpoints for parsing, matching, email
  components/               # UI + workflow components
  lib/                      # services, validators, utils, auth helpers
  stores/                   # Zustand workflow store
```

Additional documentation:

- `docs/architecture.md` – high-level system plan and data model.

## Getting Started

### 1. Install dependencies

```bash
npm install
```

### 2. Configure environment

Create `.env.local` (or set variables in Vercel) based on `.env.example`:

| Variable | Required | Description |
|----------|----------|-------------|
| `NEXT_PUBLIC_SUPABASE_URL` | ✅ | Supabase project URL |
| `NEXT_PUBLIC_SUPABASE_ANON_KEY` | ✅ | Supabase anon key for browser client |
| `SUPABASE_SERVICE_ROLE_KEY` | optional | Enables server-side persistence endpoints |
| `SUPABASE_DB_PASSWORD` | optional | Required for running SQL migrations locally |
| `RESUME_EXPORT_SECRET` | optional | Use to sign export links when adding PDF automation |
| `OPENAI_API_KEY` | optional | Enables AI-written recruiter emails (falls back if missing) |
| `EMAIL_PROVIDER_API_KEY` | optional | Placeholder for sending email via third-party |

### 3. Set up Supabase

1. Create a new Supabase project.
2. Run SQL migrations (manual for now) to create tables outlined in `docs/architecture.md` (`resumes`, `job_posts`, `matches`, `email_templates`).
3. Enable Row Level Security and default policies (`user_id = auth.uid()`).
4. Configure authentication (email OTP) and SMTP sender for production magic links.
5. Add the anon key and service role key to your environment variables.

### 4. Run the dev server

```bash
npm run dev
```

Routes to explore:

- `/` – marketing overview
- `/auth` – magic link sign-in form
- `/app` – full workflow canvas (requires Supabase session or comment out middleware during local prototyping)

## Workflow Overview

1. **Ingest** – paste raw resume text, parse to structured schema, and refine via editor.
2. **Tailor** – adjust resume sections side-by-side with the target job description.
3. **Match** – compute keyword/semantic scores and inspect recommended highlights.
4. **Email** – capture recruiter details, generate outreach draft, optionally powered by OpenAI.
5. **Export** – download ZIP bundle (`resume.json`, `job.json`, `match.json`, `email.txt`) or copy the outreach email.

State is managed inside `useWorkflowStore` (Zustand) so the UI remains reactive and simple to extend.

## API Surface

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/api/resume/parse` | `POST` | Convert raw text to the normalized resume schema |
| `/api/resume/tailor` | `POST` | Tailor resume to job and recompute match metrics |
| `/api/matches` | `POST` | Persistable match scoring (requires Supabase session) |
| `/api/email` | `POST` | Generate recruiter outreach draft; persists when requested |

All request/response contracts are validated via Zod schemas under `src/lib/validators`.

## Deployment Checklist (Vercel + Supabase)

1. **Push to GitHub** – connect the repository to Vercel.
2. **Configure Environment Variables** – set the variables listed above inside the Vercel project dashboard (`Project Settings → Environment Variables`).
3. **Enable Supabase Auth Redirects** – add `https://<your-vercel-domain>/auth/callback` to Supabase Auth redirect URLs.
4. **Run Database Migrations** – execute the SQL scripts or use Supabase Studio to create tables before first deploy.
5. **Deploy** – Vercel will handle builds automatically; preview deployments mirror production env configuration.
6. **Post-deploy** (optional) – connect custom domain, enable Supabase Storage buckets for future resume uploads, configure cron or edge functions for scheduled follow-ups.

## Extending the Platform

- Integrate PDF rendering via Playwright or `@react-pdf/renderer` in `/api/export`.
- Add Kanban or calendar views for outreach follow-up using Supabase realtime.
- Wire external job feeds to seed `job_posts` and auto-suggest tailoring instructions.
- Gate AI features behind feature flags stored in Supabase.

## Commands

| Script | Purpose |
|--------|---------|
| `npm run dev` | Start development server |
| `npm run build` | Create production build |
| `npm run start` | Run production server |
| `npm run lint` | Run ESLint across the project |

## Support

- Architecture & roadmap: `docs/architecture.md`
- Deployment policies: see **Deployment Checklist** above
- Issues and enhancements: track via your connected Git hosting provider

Happy tailoring ✨
# Resume-Parser
# Resume-Parser
# Resume-Parser
