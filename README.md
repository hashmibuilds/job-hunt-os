# Job Hunt OS v0.3

Standalone Job Hunt Command Center connected to Supabase.

## Included
- Supabase Auth (email/password)
- Live Supabase reads/writes for profile, jobs, applications, interviews, skills and resumes
- Approval-first application pipeline
- Separate Job Fit and Career Trajectory scoring
- Manual job intake
- One-active-skill priority
- Zaffar assistant using stored Job Hunt OS data
- Browser-safe Supabase publishable key only

## Run
Open `index.html` in a browser. Internet access is required for the Supabase JS CDN and Supabase API.

If Supabase email confirmation is enabled, confirm the signup email before signing in.

## Security
The frontend uses only the Supabase publishable key. Never put a service-role/secret key in this folder or browser code.

## Current implementation status
- Job discovery: prototype public-search discovery + canonical Supabase ingestion; multi-source production crawler still pending
- AI JD ↔ resume analysis: deployed `job-ai-intelligence` with LLM-provider fallback (OpenRouter/OpenAI/Anthropic/Gemini when configured) plus deterministic fallback; web results are treated as research leads
- Company/recruiter research: research page + saved notes; automated live research worker still pending
- Resend application send: deployed and approval-first
- Personal career digest: deployed `job-notification-digest`; manual send from Career Intelligence
- Follow-up worker: deployed; currently manually invoked from UI, no scheduled worker yet
- hosted deployment / custom domain

## v0.8 workflow hardening
- Added server-side application transition RPC with validated state machine and event logging.
- Added owner-only email approval RPC.
- Added follow-up scheduling RPC with max-attempt protection.
- Added approval/send controls in the UI; send requires explicit confirmation and invokes job-application-send.
- Manual job intake now uses the canonical jh_ingest_job RPC.
- Profile writes now map to the actual profile schema.
- Job scoring now passes an active resume when available.
- Added job-email-health Edge Function to verify Resend secret configuration from an authenticated session without exposing the API key.

## v1.9 discovery upgrade
- Fixed the job-card rendering path so the Job Discovery page renders reliably.
- Discovery now targets public ATS result patterns (Greenhouse, Lever, Workday, Ashby) in addition to general public web search.
- Each discovery result carries source provenance (`source_name` / `source_type`) where detectable.
- Newly ingested opportunities are automatically analyzed for the first five returned jobs when an authenticated AI analysis path is available; deterministic fallback remains supported.
- Discovery remains approval-safe: finding/analyzing jobs never sends recruiter/application emails.
- This is still a public-search discovery adapter, not a claim of unrestricted scraping of LinkedIn/Indeed/Naukri. Terms, robots, CAPTCHA and access controls must be respected.
