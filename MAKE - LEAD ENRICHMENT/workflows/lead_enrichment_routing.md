# Workflow: Lead Enrichment & Routing (Make.com)

## Objective
Turn every inbound form lead into a scored, routed, logged record with zero manual triage:
qualify with AI, log all leads, escalate Hot ones to sales, auto-acknowledge Warm ones.

## Trigger
A **custom webhook** receives `POST` JSON `{ name, email, company, message }` from any web form.

## Inputs / config to set per deployment
- **Gemini API key** — in the HTTP module's `x-goog-api-key` header (free key from Google AI Studio).
- **Google Sheet ID** — replace `REPLACE_WITH_SHEET_ID`; sheet needs a `Leads` tab with columns
  `date, name, company, email, tier, score, reason`.
- **Slack channel** — default `#sales-hot-leads`.
- **Email connection** — SMTP/Gmail for the Warm auto-reply.

## Steps (modules)
1. **Webhook** — receive the lead.
2. **HTTP → Gemini** (`gemini-flash-latest:generateContent`) — one call returns compact JSON
   `{ tier: Hot|Warm|Cold, score: 0–100, reason, reply }`.
3. **Parse JSON** — turn the model's text into structured fields.
4. **Google Sheets → Add a row** — append every lead (audit trail / history).
5. **Router**:
   - `tier = Hot` → **Slack** message to sales.
   - `tier = Warm` → **Email** the AI-drafted `reply`.
   - `tier = Cold` → no route matches; the lead is already logged in step 4.

## Expected output
- One new row in the Leads sheet per submission.
- A Slack ping for Hot leads; an acknowledgement email for Warm leads.

## Edge cases & how they're handled
- **Model returns malformed JSON** → the HTTP module has `handleErrors` on; add a fallback route that
  logs the lead as `tier = Cold, score = 0` so nothing is dropped. (Extend the router with a catch-all
  filter if you want this explicit.)
- **Missing form fields** → Gemini still scores on whatever is present; empty `email` skips the Warm
  send (add a filter `email exists` on the Email module).
- **Duplicate submissions** → optionally add a Sheets *Search Rows* before Add Row to dedupe on email.
- **Make module-version prompts on import** → expected; re-confirm the app version, not a real error.

## Cost notes
- One Gemini free-tier call per lead. Make operations scale with lead volume — the free Make plan
  suits low volume; heavy volume needs a paid plan.

## Reuse
Retarget to a client by changing the Sheet ID, the Slack channel, and (optionally) the qualifier
prompt's definition of what counts as "Hot". The flow itself is unchanged.
