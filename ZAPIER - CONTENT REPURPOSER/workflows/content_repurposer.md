# Workflow: Content Repurposer (Zapier)

## Objective
Turn every new blog post into a ready-to-review multi-channel package automatically: the moment a post
publishes, Gemini rewrites it into a LinkedIn post, an X/Twitter thread, and a newsletter blurb, each
saved as its own Google Doc draft, the run logged for reporting, and a Slack ping raised — while a
human still edits and publishes every channel.

## Trigger
**RSS by Zapier → New Item in Feed** on the blog/newsletter feed URL.

## Inputs / config to set per deployment
- **RSS feed URL** — the blog's feed in the trigger.
- **Gemini API key** — in the Webhooks-by-Zapier POST header `x-goog-api-key` (free from Google AI Studio).
- **Google Docs destination** — the Drive folder the three draft docs are created in.
- **Google Sheet** — columns `date, source_title, link, status`.
- **Slack channel** — default `#content`.
- **Channels** — edit the channel list / tone in the step-2 prompt (`linkedin`, `x_thread`, `newsletter`).

## Steps
1. **RSS trigger** — new item in the feed (`title`, `link`, `content`).
2. **Webhooks by Zapier → POST Gemini** — returns `{ linkedin, x_thread, newsletter }`.
3. **Formatter / Code (optional)** — parse the JSON string into fields.
4. **Google Docs → Create Document from Text** — LinkedIn post, doc `LinkedIn — {{title}}`. **Draft.**
5. **Google Docs → Create Document from Text** — X/Twitter thread, doc `X thread — {{title}}`. **Draft.**
6. **Google Docs → Create Document from Text** — newsletter blurb, doc `Newsletter — {{title}}`. **Draft.**
7. **Google Sheets → Create Row** — log the run (`status = drafted`).
8. **Slack** — post "3 drafts ready for {{title}}" + the three doc links to `#content`.

## Expected output
- Three ready-to-review Google Doc drafts (LinkedIn / X / newsletter) per new post.
- One Sheet row per post; one Slack ping linking the drafts.

## Edge cases & how they're handled
- **Model returns malformed JSON** → add Formatter defaults so each channel still gets whatever text
  came back (never a blank doc); worst case one doc holds the raw model output for a human to split.
- **Empty or truncated RSS `content`** (feed only ships a summary) → fall back to `title` +
  `description` in the step-2 prompt; the drafts are shorter but still usable.
- **Duplicate/edited posts re-triggering** → RSS de-dupes by item GUID; if a feed re-publishes, add a
  Filter step on `link` so the same post isn't repurposed twice.
- **Zapier plan limits** → steps 4–8 make this a multi-step Zap (paid plan). A stripped 2-step version
  (RSS trigger → one Google Doc holding all three channels) runs on the free plan.

## Cost notes
- One Gemini free-tier call per new post. Zapier task usage scales with publishing frequency and plan tier.

## Reuse
Retarget by changing the RSS feed, the Docs folder, the Sheet, and the Slack channel. Edit the channel
list and tone in the step-2 prompt (drop newsletter, add an Instagram caption, etc.) without changing
the wiring. The draft-for-human-review step is the deliberate safety design — keep it.
