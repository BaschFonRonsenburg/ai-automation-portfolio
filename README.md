# AI Automation — Portfolio

I build **AI-powered automations** that do real work: scoring and routing leads, triaging support,
turning a single URL into a competitor analysis, and delivering designed reports straight to an inbox,
a spreadsheet, Slack, or Teams. Every project here runs on **free-tier APIs** (no scraping, no paid
models required to try it), keeps a human in the loop where it matters, and ships with a clear README
so you can see exactly what it does.

**What I do:** connect the tools a business already uses (Gmail, Sheets, Slack, Teams, Excel, a website
form) and put an LLM in the middle to handle the judgment — qualify, summarize, draft, categorize — so
the repetitive work runs itself.

---

## Featured projects

### 🖥️ PC Build Advisor — *AI chatbot*
Tell it a budget and use-case; it returns a full PC build as a side-by-side **Intel/NVIDIA vs AMD**
table with prices, picks, store links, and an upgrade path. Streamlit + Google Gemini, prices in your
local currency.

![PC Build Advisor](assets/pc-build-advisor.svg)

**Stack:** Python · Streamlit · Google Gemini &nbsp;•&nbsp; **Repo:** https://github.com/BaschFonRonsenburg/pc-build-advisor

---

### 📬 Weekly AI-Automation Job Report — *n8n, scheduled*
Runs twice a week, pulls remote AI/automation part-time roles from three job APIs, scores each employer
for legitimacy (a 0–100 **Trust score**), writes a one-line AI summary per role, and emails a **designed
HTML report** with a Trust-score chart and a styled spreadsheet attached.

![Weekly Job Report](assets/weekly-job-report.svg)

**Stack:** n8n · Google Gemini · Gmail · multi-source job APIs &nbsp;•&nbsp; **Repo:** https://github.com/BaschFonRonsenburg/n8n-ai-automation-job-report

---

### 📊 Competitive Edge Campaign Builder — *n8n, on-demand*
Submit one company URL. It reads the site, discovers real competitors, analyzes the **gaps**, writes a
prioritized action plan **and** a ready-to-post ad campaign, then emails a **themed PDF** that
re-skins to match the company's industry. Everything is a draft for human review.

![Competitive Edge Campaign](assets/competitive-edge.svg)

**Stack:** n8n · Google Gemini · DuckDuckGo · PDFShift · Gmail &nbsp;•&nbsp; **Repo:** https://github.com/BaschFonRonsenburg/marketing-automation

---

## Also built on — platform breadth

The same automation approach, shown on the other major no-code platforms. Each is a self-contained demo
with its own README, an importable/reference definition, and a mockup.

| Platform | Demo | What it does |
|---|---|---|
| **Make.com** | [Lead Enrichment & Routing](MAKE%20-%20LEAD%20ENRICHMENT/) | Form → AI qualify + score → log every lead → route Hot to Slack, Warm to auto-reply |
| **Zapier** | [Support Email Triage](ZAPIER%20-%20SUPPORT%20TRIAGE/) | New email → AI categorize + draft reply → **Gmail draft a human approves** → log + Slack |
| **Power Automate** | [Weekly Operations Digest](POWER%20AUTOMATE%20-%20OPS%20DIGEST/) | Mon 07:00 → read Excel table → AI summary → post to Teams + email the ops leads |

---

## Capabilities

- **Automation platforms:** n8n (deep — self-hosted & cloud, custom code nodes) · Make.com · Zapier ·
  Microsoft Power Automate
- **AI / LLM:** Google Gemini, prompt design, structured JSON output, batching for free-tier limits,
  human-in-the-loop review
- **Apps & delivery:** Gmail / Outlook · Google Sheets / Excel · Slack / Teams · webhooks & forms ·
  designed HTML email · PDF reports · styled spreadsheets
- **Also:** Python · Streamlit chatbots · web scraping/enrichment · scheduled & event-driven workflows

## How I work

Each automation is built so a non-technical owner can run it: clear README, credentials kept out of the
code (bring your own keys), free-tier friendly, and **retargetable to a new client by changing a field
or two**. Anything that touches a customer is a **draft for human approval** unless you ask otherwise.

## Contact

**Ryan** — haban.johnryandc@gmail.com

<sub>Project previews are illustrative mockups; each project's README explains how to render real output.
The Make/Zapier/Power Automate demos live in this folder; the three featured projects are public GitHub
repos linked above.</sub>
