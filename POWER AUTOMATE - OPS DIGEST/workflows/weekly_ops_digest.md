# Workflow: Weekly Operations Digest (Power Automate)

## Objective
Produce the weekly ops status update automatically: read a metrics table, have AI summarize
week-over-week performance, and deliver the digest to Teams and email — no one writes it by hand.

## Trigger
**Recurrence** — every Monday 07:00 (set your `timeZone`).

## Inputs / config to set per deployment
- **Excel workbook + table** — `OpsMetrics` table with columns `Metric, ThisWeek, LastWeek`
  (replace `REPLACE_WITH_WORKBOOK_ID`).
- **Gemini API key** — in the HTTP action header `x-goog-api-key` (free from Google AI Studio).
- **Teams channel id** — replace `REPLACE_WITH_TEAMS_CHANNEL_ID`.
- **Email recipients** — the ops distribution list.

## Steps (actions)
1. **Recurrence** — Monday 07:00.
2. **Excel Online → List rows present in a table** (`OpsMetrics`).
3. **Select** — build one text line per metric (`"Signups: 412 (prev 380)"`).
4. **HTTP → Gemini** (`gemini-flash-latest:generateContent`) — returns
   `{ headline, wins[], risks[], actions[] }`.
5. **Parse JSON** — structure the summary.
6. **Teams → Post card** to the ops channel.
7. **Office 365 Outlook → Send an email (V2)** — same digest to ops leads.

## Expected output
- A Teams card and an email every Monday morning with headline, wins, risks, and recommended actions.

## Edge cases & how they're handled
- **Empty table / no rows** → add a Condition after step 2 to post "no data this week" instead of calling
  the model.
- **Model returns malformed JSON** → Parse JSON will fail the run; add a `runAfter` fallback action that
  posts the raw model text so the digest still goes out, or set the Parse schema fields as optional.
- **HTTP action unavailable (non-premium plan)** → swap step 4 for AI Builder's *Create text with GPT*,
  or call Gemini via an Azure Function (see README premium note).
- **Time zone drift** → the Recurrence `timeZone` is explicit; change it per client region.

## Cost notes
- One Gemini free-tier call per week (negligible). The real cost gate is the **premium HTTP connector**
  in Power Automate — see the README for non-premium alternatives.

## Reuse
Retarget by changing the workbook/table, the Teams channel, the recipients, the schedule, and the
summary prompt's definition of wins/risks. Structure is unchanged.
