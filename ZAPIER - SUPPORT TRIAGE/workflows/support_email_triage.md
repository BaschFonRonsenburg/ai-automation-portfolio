# Workflow: Support Email Triage (Zapier)

## Objective
Clear the support inbox backlog: every incoming support email is categorized and given a **drafted**
reply automatically, logged for reporting, and escalated to Slack when urgent — while a human still
approves and sends every customer-facing message.

## Trigger
**Gmail → New Email Matching Search** on `label:support` (or `to:support@yourco.com`).

## Inputs / config to set per deployment
- **Gemini API key** — in the Webhooks-by-Zapier POST header `x-goog-api-key` (free from Google AI Studio).
- **Google Sheet** — columns `date, from, subject, category, priority, status`.
- **Slack channel** — default `#support`.
- **Categories** — edit the list in the step-2 prompt (`Billing|Technical|Account|Other`).

## Steps
1. **Gmail trigger** — new support email.
2. **Webhooks by Zapier → POST Gemini** — returns `{ category, priority, reply }`.
3. **Formatter / Code (optional)** — parse the JSON string into fields.
4. **Paths by Zapier** — `priority = Urgent` branch also runs the Slack step.
5. **Gmail → Create Draft Reply** — draft addressed to the sender with the AI `reply`. **Not sent.**
6. **Google Sheets → Create Row** — log the ticket (`status = drafted`).
7. **Slack (urgent path only)** — post subject + priority to `#support`.

## Expected output
- A ready-to-review Gmail **draft** for every support email.
- One Sheet row per email; a Slack ping for urgent ones.

## Edge cases & how they're handled
- **Model returns malformed JSON** → add a Formatter default so `category = Other, priority = Normal`;
  the draft still gets created from whatever `reply` text came back.
- **Non-support email slips through** → tighten the Gmail search; optionally add a Filter step after the
  trigger.
- **No reply address / auto-responder loop** → the draft is never auto-sent, so there's no loop risk;
  a human decides whether to send.
- **Zapier plan limits** → Paths and 3+ step Zaps require a paid Zapier plan; a stripped 2-step version
  (trigger → draft) runs on the free plan.

## Cost notes
- One Gemini free-tier call per email. Zapier task usage scales with email volume and plan tier.

## Reuse
Retarget by changing the trigger search, the Sheet, the Slack channel, and the category list in the
step-2 prompt. The draft-for-human-approval step is the deliberate safety design — keep it.
