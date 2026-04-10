---
type: resource
resource-subtype: technical
created: 2026-03-19
category: GTM
tool-type: ai-agent-builder
shareability: internal
status: active
tags:
  - clay
  - gtm
  - ai-agents
---

# AutoClaygent

**What it does:** AI-guided workflow for building production-quality Claygent prompts using Claude Code. Created by Jordan Crawford (Blueprint GTM). You define what data you need, it walks you through discovery, prompt drafting, batch testing against Clay's webhook API, and iterative scoring until you hit production quality.

**Website:** https://autoclaygent.blueprintgtm.com
**Repo:** https://github.com/blueprintgtm/AutoClaygent
**Local clone:** `Tools/Enrichment/Clay/AutoClaygent/`
**Premium content:** `Tools/Enrichment/Clay/AutoClaygent/premium-content/`

## Contents

- [[#How It Works]]
- [[#Architecture]]
- [[#Evaluation Rubric]]
- [[#Core Prompt Engineering Principles]]
- [[#Nine Production Patterns]]
- [[#Common Prompt Templates]]
- [[#Model Selection]]
- [[#Worked Examples]]
- [[#Anti-Patterns to Avoid]]
- [[#Clay Webhook Payload]]
- [[#JSON Schema Constraints]]
- [[#Setup Requirements]]
- [[#Where It Fits]]

---

## How It Works

AutoClaygent is a **Claude Code project** (not a standalone app). You open the repo in Claude Code and the `CLAUDE.md` file loads a multi-mode workflow:

1. **Mode 0-2:** Setup -- license validation, git updates, environment check
2. **Mode 3:** Fetches premium workflow content from Blueprint GTM's API (watermarked per license)
3. **Mode 4:** Configures Supabase backend -- database table, Edge Function for Clay callbacks, round-trip test
4. **Mode 5:** Build mode -- the actual Claygent development loop

### Build Loop (Mode 5)

```
Discovery → Native Integration Check → Draft Prompt → Send Test Batch to Clay →
Receive Results via Supabase → Score Against Rubric → Iterate Until 8.0+ → Deploy
```

- **Discovery:** Define inputs (domain, company name, etc.), desired outputs, edge cases, JSON schema
- **Native integration check:** Uses `/clay-integrations` skill to see if Clay already has a built-in integration (cheaper, faster than a custom Claygent). Three choices: native only, custom Claygent, or hybrid (native first with Claygent fallback)
- **Testing:** Sends batches of 25-50 rows to Clay via webhook, results callback to Supabase Edge Function
- **Evaluation:** Scores results on 7 weighted dimensions against the rubric
- **Target:** 8.0+ overall score before deploying to production

---

## Architecture

Two HTTP endpoints form the round-trip:

1. **Claude → Clay:** Claude sends prompts to Clay's webhook URL via `curl POST`
2. **Clay → Supabase:** After the Claygent runs, Clay's HTTP Callback column POSTs results to your Supabase Edge Function
3. **Supabase → Claude:** Claude queries the `claygent_results` table via Supabase MCP to pull results back into the conversation

```
Claude Code                          Clay Template Workbook
    │                                    │
    │ curl POST (prompt + batch_id       │
    │   + callback_url + JSON schema)    │
    ├───────────────────────────────────→ Webhook Receiver column
    │                                    │
    │                                    ↓
    │                                Claygent column (runs prompt)
    │                                    │
    │                                    ↓
    │                                JSON Parser column
    │                                    │
    │                                    ↓
    │                                HTTP Callback column
    │                                    │
    │                                    │ POST (results JSON)
    │                                    ↓
    │                          Supabase Edge Function
    │                          (claygent-webhook)
    │                                    │
    │                                    ↓
    │                          PostgreSQL table
    │                          (claygent_results, 24hr TTL)
    │                                    │
    │  MCP query: SELECT * FROM          │
    │  claygent_results WHERE            │
    │  batch_id = '...'                  │
    ←────────────────────────────────────┘
    │
    ↓
Score against rubric → iterate or ship
```

### Clay Template Workbook

You copy a pre-built template (`https://app.clay.com/shared-workbook/share_0t8b7fh5xNR57ptm4R2`) into your Clay workspace. It has 4 columns pre-wired:

| Column | Type | Purpose |
|--------|------|---------|
| Webhook Receiver | Source | Catches incoming data from Claude's `curl` calls |
| Claygent | AI Agent | Runs whatever prompt arrived in the payload |
| JSON Parser | Formula | Extracts structured data from Claygent response |
| HTTP Callback | Action | POSTs results back to your Supabase Edge Function |

The template is reusable across projects -- you don't need a new one for each Claygent. AutoClaygent sends different prompts to the same webhook each time.

### Supabase Backend

Claude deploys this automatically during Mode 4 setup using MCP tools:

- **Edge Function** (`claygent-webhook`) -- Deno serverless function that receives Clay's HTTP callbacks and writes them to the database. JWT verification is disabled since Clay can't send auth headers.
- **Database table** (`claygent_results`) -- stores batch_id, row_id, prompt_version, claygent_output, raw_payload. Rows auto-expire after 24 hours via `expires_at` column.
- **RLS policies** -- anonymous insert allowed (for Clay callbacks), select restricted to non-expired rows.

Replaces the old approach of SSH-tunneling to a local Flask server (`webhook_server.py`, now deprecated). Supabase is always up -- no more tunnel drops.

### Key Files in Repo

- `CLAUDE.md` -- orchestrates the entire 5-mode workflow
- `webhook_server.py` -- legacy local Flask server (deprecated)
- `references/` -- Clay JSON rules, template format, integration docs
- `projects/` -- per-project folders with prompts, test data, results

### Config (`~/.claygent-builder/`)

- `license.key` -- CMB-xxxxx format
- `supabase_project_id` -- which Supabase project to use
- `callback_url` -- your Supabase Edge Function URL (never changes once set)
- `current_batch_id` -- session-scoped batch grouping, regenerated each session

---

## Evaluation Rubric

Seven weighted dimensions for scoring Claygent output quality (0-10 scale):

| Dimension | Weight | What It Measures |
|-----------|--------|------------------|
| **Accuracy** | 25% | Is the extracted information correct? Verified against authoritative sources |
| **Completeness** | 25% | Were all requested fields populated? (Legitimately null fields don't penalize) |
| **JSON Validity** | 15% | Valid JSON matching the schema, correct types, enum values, no extras |
| **Source Quality** | 15% | Did the Claygent use authoritative sources? Primary > Secondary > Tertiary |
| **Step Efficiency** | 10% | Did it take an efficient path? 1-3 steps = 10, 8+ steps = 2 |
| **Consistency** | 5% | Same input produces same output across runs |
| **Error Handling** | 5% | Graceful failures -- null + low confidence + reason, not wrong data |

**Quality thresholds:**
- **8.0+** = Production ready -- deploy to full table
- **7.0-7.9** = Almost ready -- one more iteration
- **5.0-6.9** = Needs work -- 2-3 iterations
- **<5.0** = Major issues -- significant redesign

**Source quality tiers:**
- **Primary (10):** Company website, official LinkedIn
- **Secondary (8):** Crunchbase, G2, SEC filings
- **Tertiary (5):** News articles, press releases
- **Low (2):** Forums, user-generated content
- **None (0):** No source cited

---

## Core Prompt Engineering Principles

### The 3-Task Rule

Claygents work best doing **3 or fewer distinct tasks** per prompt. For complex research, split into multiple Claygent columns:

- **Column 1:** Basic company info (size, industry)
- **Column 2:** Tech stack detection
- **Column 3:** Funding/news research

### Be Specific, Not Clever

Name exact sources and pages to check. Never say "search the web."

```
BAD:  "Research the company thoroughly"
GOOD: "1. Visit /about or /team page. 2. Check LinkedIn company page.
       3. Look for press releases on PRNewswire"
```

### Handle Failures Gracefully

Every prompt MUST include:
- What to do if primary source fails
- Fallback sources to check
- Default values for optional fields (null, not guessed)
- Confidence field (high/medium/low) on every output

### URL Patterns Beat Text Scraping

| Detection Method | Accuracy |
|-----------------|----------|
| Subdomain match (company.platform.com) | 95%+ |
| CNAME redirect | 90%+ |
| SSL certificate inspection | 85%+ |
| Iframe embed | 75% |
| Text mention on page | 70% |

### Null Is a Feature

Returning null when unsure **always** beats returning wrong data with high confidence. "Unverifiable" is a valid answer.

---

## Nine Production Patterns

AutoClaygent includes 9 battle-tested Claygent patterns (with complete prompts and JSON schemas in `premium-content/patterns.md`):

### 1. Business Model Classification
Classify companies by revenue model (subscription, transactional, hybrid, freemium, enterprise). Uses explicit decision trees based on /pricing page analysis. Confidence: HIGH = explicit pricing page, MEDIUM = inferred, LOW = guessing.

### 2. Platform Detection via URL Patterns
Detect SaaS platforms from portal/login URLs. Check domain patterns like `{company}.platform.com`, redirects, and widgets. 95%+ accuracy because DNS doesn't lie.

### 3. CRM Validation & Mismatch Detection
Find where **existing CRM data is wrong** -- not just adding new data, but comparing CRM values against website reality. Statuses: MATCH, MISMATCH, STALE, UNVERIFIABLE. Rule: can't find info = "unverifiable", NOT "match."

### 4. Digital Maturity Profiling
Score prospects by online sophistication using ONLY email/phone (no website visit needed). Segments: digital_dabbler, digitally_active, digitally_mature.

### 5. B2B vs B2C Segment Detection
Primary signal: check navigation menu first (B2B keywords: "Enterprise", "Partners"; B2C: "Shop", "Families"). Absence of B2B signals does NOT mean B2C -- use "unknown."

### 6. Corporate Structure Detection
Standalone, subsidiary, franchise, PE-backed, or holding company. Check footer for "A [Parent] company", /about for acquisitions, multiple locations for franchise. Affects who the decision-maker is.

### 7. Buying Intent Detection
Find companies ready to buy NOW. Signals: recent funding (<6 months), leadership changes, expansion, hiring spree. Scoring: 80-100 = HOT, 60-79 = WARM, 40-59 = NEUTRAL, 0-39 = COLD.

### 8. Multi-Axis Classification Engine
Classify along independent dimensions (size, business model, geography, tech sophistication, market position). Each scored independently with confidence. Allows nuance -- SMB on size but "Enterprise" on tech stack.

### 9. SMB Owner Discovery Pipeline
Find small business owner using waterfall: Google Maps → website team page → contact/footer → social media → web search. Confidence stacking: 1 source = LOW (50%), 2 sources = MEDIUM (75%), 3+ = HIGH (90%+).

### Pattern Combinations

| Workflow | Patterns | Use Case |
|----------|----------|----------|
| **Full Company Profile** | 1 + 5 + 6 + 8 | Comprehensive account enrichment |
| **Sales Timing** | 7 + 3 + 6 | Find ready-to-buy accounts |
| **SMB Outreach** | 4 + 9 + 1 | Local business prospecting |
| **Tech Stack Intel** | 2 + 8 + 7 | Competitive displacement campaigns |

---

## Common Prompt Templates

### Direct Lookup
Single-source extraction for simple facts.

```
Find the {field} for {company} by visiting their {specific_page}
```

### Multi-Source Waterfall
When primary source might fail -- try sources in order until one works.

```
1. Check source A first
2. If not found, check source B
3. If still not found, check source C
4. If all sources fail, return null with low confidence
```

### Conditional Research
Different paths based on company characteristics.

```
If company is startup (<50 employees):
  - Check Crunchbase, AngelList
If company is enterprise (>500 employees):
  - Check official website leadership page
  - Verify via SEC filings
```

### Verification Chain
When accuracy is critical -- cross-reference multiple sources.

```
1. Find potential {field} from source A
2. Cross-reference against source B
3. Only report if both sources agree
4. If sources conflict, report both with "unverified" confidence
```

---

## Model Selection

| Model | Best For | Cost | When to Use |
|-------|----------|------|-------------|
| **GPT-5 Nano** | Most tasks | ~$0.0005/row | Default choice, best value |
| **GPT-5 Mini** | Structured extraction | ~$0.001/row | Strong default, multi-field output |
| **Claude Haiku 4.5** | Fast + accurate | ~$0.001/row | Quick lookups, simple extraction |
| **GPT-5** | Complex reasoning | ~$0.01/row | Multi-step logic, ambiguous data |
| **Claude Sonnet 4.6** | Deep analysis | ~$0.01/row | Long-form content, nuanced research |

**BYOK is the recommended approach** for production workflows. Clay's built-in models (Helium, Neon, Argon) are convenient for getting started but BYOK gives better cost-per-quality at scale. See [[Claygent Guide#Clay Models vs BYOK]].

---

## Worked Examples

Three complete end-to-end examples in `premium-content/`, each showing the full build cycle (problem → native check → prompt drafting → testing → evaluation → iteration → production):

### Platform Detection (`example-tech-stack.md`)
Detecting what scheduling/booking platform a service business uses by analyzing portal and login URLs. Demonstrates URL pattern detection beating text scraping. V1 scored 9.0/10, V2 hit 9.6/10 after adding platform disambiguation rules and SSL certificate inspection.

### SMB Owner Discovery (`example-contact-discovery.md`)
Finding small business owner names from a list of 5,000 local service businesses (plumbers, electricians). Multi-stage waterfall approach since enterprise tools (Apollo, PDL) miss SMB. Demonstrates progressive enrichment across Google Maps, website, social, and web search.

### CRM Validation (`example-company-research.md`)
Validating 50,000 CRM accounts where sales says industry, company size, and business type fields are wrong. Compares CRM data against website reality and flags mismatches. Demonstrates that mismatch detection (finding errors) is often more valuable than adding new data.

---

## Anti-Patterns to Avoid

- **Vague instructions** -- "Research the company thoroughly" gives inconsistent results
- **Too many tasks** -- more than 3 per prompt degrades quality
- **No output format** -- always specify exact JSON schema
- **No failure handling** -- prompts without fallbacks crash on edge cases
- **Generic source guidance** -- "search the web" is useless; name exact URLs
- **Using Clay's built-in models** -- always BYOK (GPT-4o-mini or Claude Haiku)
- **Trusting all data** -- always include confidence scoring and verification steps

---

## Clay Webhook Payload

Every request to Clay must include exactly 7 fields:

```json
{
  "row_id": "unique_identifier",
  "batch_id": "session_20260113_143000_12345",
  "prompt": "Full Claygent prompt with embedded data",
  "prompt_version": "detector-v1.0",
  "change_log": "What changed from last version",
  "callback_url": "https://[PROJECT].supabase.co/functions/v1/claygent-webhook",
  "reference_json_text": "JSON schema as text for Clay's structured output"
}
```

All input data must be **embedded directly in the prompt text** -- Clay Claygents don't support separate input fields.

---

## JSON Schema Constraints

Clay uses OpenAI Structured Outputs, which imposes strict rules:

- Root must be a single object `{}`
- Allowed types: `string`, `number`, `integer`, `boolean`, `object`, `array`, `null`, `enum`
- **Forbidden:** `oneOf`, `allOf`, `not`, `if/then/else`, `pattern`, `minLength`, `format`, `const`, `default`
- Max 100 properties, 5 levels deep
- All objects require `"additionalProperties": false`
- Optional fields use `anyOf` with null: `{"anyOf": [{"type": "string"}, {"type": "null"}]}`

---

## Setup Requirements

1. **License key** -- purchased from Blueprint GTM (one-time), stored at `~/.claygent-builder/license.key` and backed up in `Tools/Enrichment/Clay/.env`
2. **Claude Code** -- the CLI tool, not Claude Desktop
3. **Supabase account** -- free tier, no credit card required
4. **Supabase MCP** -- configured in `~/.claude/mcp.json`:

```json
{
  "mcpServers": {
    "supabase": {
      "command": "npx",
      "args": ["-y", "@supabase/mcp-server-supabase@latest"],
      "env": {
        "SUPABASE_ACCESS_TOKEN": "sbp_YOUR_TOKEN"
      }
    }
  }
}
```

5. **Clay webhook URL** -- from shared workbook template: `https://app.clay.com/shared-workbook/share_0t8b7fh5xNR57ptm4R2`

---

## Where It Fits

Sits **on top of Clay** as a prompt engineering and testing harness. Instead of manually writing Claygent prompts and eyeballing results in Clay's UI, AutoClaygent gives you a structured build-test-iterate loop with scoring rubrics and version tracking. Useful when building complex enrichment agents that need to handle edge cases reliably at scale.

Not a replacement for Clay -- it's a development tool for building better Claygents. Once your prompt scores 8.0+, you deploy it directly in Clay.

---

Sources: [Blueprint GTM](https://blueprintgtm.com), [AutoClaygent](https://autoclaygent.blueprintgtm.com), [Jordan Crawford LinkedIn](https://linkedin.com/in/jordancrawford), [Cannonball GTM Newsletter](https://cannonballgtm.substack.com)
Related: [[Claygent Guide]], [[Claygent Recipes]], [[Prompt Implementation Rules]]

*Last updated: 2026-03-19*
