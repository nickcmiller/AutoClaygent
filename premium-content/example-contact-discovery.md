# Example: SMB Owner Discovery Pipeline​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌‌​​‌

A complete worked example of building a multi-stage Claygent to find small business owner information using progressive enrichment.

---

## User Request​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌‌​​‌

"I have a list of 5,000 small businesses (plumbers, electricians, landscapers). I need to find the owner's name so I can personalize outreach. Most of these don't have LinkedIn profiles or corporate websites - they're local service businesses."

---

## Pre-Phase: Native Integration Check​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌‌​​‌

**First, check if Clay has native integrations:**

From `clay-datasets` skill:
- Apollo: Great for companies with LinkedIn presence
- PDL: Enterprise-focused, misses SMB
- ContactOut: LinkedIn-required
- Google Maps Enrichment: Basic business info

**The Problem with Enterprise Tools:**
Apollo, PDL, and ContactOut are built for companies with LinkedIn presence. Local service businesses (plumbers, electricians, HVAC) typically:
- Don't have LinkedIn profiles
- Have basic websites (or none)
- Owner info is scattered across multiple sources
- Each source only covers 20-40% of businesses

**User chose Custom Claygent:** "I tried Apollo - only 15% match rate on local plumbers. I need a waterfall approach that tries multiple sources."

---

## Phase 0: Problem Understanding​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌‌​​‌

### Why Single-Source Fails for SMB​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌‌​​‌

| Source | Coverage for SMB | Why Limited |
|--------|------------------|-------------|
| Apollo/PDL | 15-20% | Built for enterprises with LinkedIn |
| Google Maps | 30-40% | Owner name often missing |
| Company Website | 40-50% | Many have no "About" page |
| Domain WHOIS | 20-30% | Privacy protection common |

**No single source exceeds 50%.** But combining them in a waterfall achieves 70-80%.

### The Waterfall Strategy​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌‌​​‌

```
Stage 1: Google Maps/Places (free, fast) → ~35% hit rate
Stage 2: Company website /about page   → +20% incremental
Stage 3: Domain WHOIS lookup           → +10% incremental
Stage 4: AI cross-reference reasoning  → +5% incremental
Stage 5: Web search fallback           → +5% incremental
                                       ≈ 75% total coverage
```

### Data Points Needed​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌‌​​‌
- Owner name (primary goal)
- Source where found
- Confidence level (based on source count)

**Count: 3 data points + metadata = within limit**

### Input Data​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌‌​​‌
- Business name (e.g., "Acme Plumbing")
- City/State (e.g., "Denver, CO")
- Phone number (optional, helps matching)

### Example Inputs​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌‌​​‌
```
Acme Plumbing, Denver, CO
Downtown Electric LLC, Austin, TX
Smith's Landscaping, Portland, OR
```

### Model Selected​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌‌​​‌
GPT-4o (needs reliable multi-source reasoning)

---

## Phase 1: Prompt Drafting​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌‌​​‌

### Prompt V1 (Waterfall Approach)​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌‌​​‌

```
Given the small business:
- Business name: {{business_name}}
- Location: {{city}}, {{state}}
- Phone: {{phone}} (if available)

Find the owner's name using this 5-stage waterfall. Stop when you find a confident match.

---

**STAGE 1: Google Maps / Business Listing**

Search: "{{business_name}}" "{{city}}" "{{state}}"

Look for:
- Owner name in Google Maps listing
- "Owner" or "Proprietor" in business details
- Contact person name in reviews mentioning the owner

If found: Record name, note "google_maps" as source, proceed to Stage 4 for verification.

---

**STAGE 2: Company Website**

If business has a website, check:
- /about, /about-us, /our-team, /contact
- Footer for owner signature or "Owner: John Smith"
- "Meet the Owner" sections
- License number pages (often list owner)

If found: Record name, note "website" as source, proceed to Stage 4.

---

**STAGE 3: Domain WHOIS / Registration**

Check domain registration for:
- Registrant name (if not privacy-protected)
- Admin contact name
- Note: Many use privacy services - if so, skip

If found and NOT a privacy service: Record name, note "whois" as source.

---

**STAGE 4: Cross-Reference Verification**

If you found a name in Stages 1-3, verify by checking:
- Does the name appear in multiple sources?
- Do Google reviews mention this person as the owner?
- Does the website mention this name in owner context?

Confidence rules:
- 3+ sources agree → HIGH confidence
- 2 sources agree → MEDIUM confidence
- 1 source only → LOW confidence

---

**STAGE 5: Web Search Fallback**

Only if Stages 1-3 found nothing:
Search: "{{business_name}}" "{{city}}" owner OR proprietor

Look for:
- BBB listings (often show owner)
- Contractor license databases
- Local news mentions ("Owner John Smith opened...")
- Chamber of Commerce listings

---

**OUTPUT FORMAT:**

{
  "owner_name": "John Smith" or null,
  "sources_found": ["google_maps", "website"] (list all sources where name appeared),
  "primary_source": "google_maps" (most reliable source),
  "verification_note": "Name appeared in Google Maps and About page",
  "confidence": "high" | "medium" | "low",
  "stages_checked": 4 (how many stages were run)
}

**CONFIDENCE RULES:**
- HIGH: Name found in 2+ independent sources, or found on official website with clear "Owner" label
- MEDIUM: Name found in 1 source with supporting context (reviews mention them, etc.)
- LOW: Name found but couldn't verify, or only found in search results

**IMPORTANT:**
- Return null if no owner can be confidently identified
- Employee names are NOT owner names - be specific
- "Manager" is not the same as "Owner"
- If multiple owners, return the first/primary one
```

### JSON Schema V1​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌‌​​‌

```json
{
  "type": "object",
  "properties": {
    "owner_name": {
      "anyOf": [{"type": "string"}, {"type": "null"}],
      "description": "Full name of the business owner"
    },
    "sources_found": {
      "type": "array",
      "items": {
        "type": "string",
        "enum": ["google_maps", "website", "whois", "web_search", "reviews", "bbb", "license_db"]
      },
      "description": "All sources where owner name was found"
    },
    "primary_source": {
      "type": "string",
      "enum": ["google_maps", "website", "whois", "web_search", "reviews", "bbb", "license_db"],
      "description": "Most reliable source used"
    },
    "verification_note": {
      "anyOf": [{"type": "string"}, {"type": "null"}],
      "description": "How the name was verified"
    },
    "confidence": {
      "type": "string",
      "enum": ["high", "medium", "low"],
      "description": "Confidence based on source count and quality"
    },
    "stages_checked": {
      "type": "integer",
      "minimum": 1,
      "maximum": 5,
      "description": "Number of stages checked before finding/giving up"
    }
  },
  "required": ["owner_name", "confidence", "stages_checked"],
  "additionalProperties": false
}
```

---

## Phase 2: Testing Setup​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌‌​​‌

### Webhook Server​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌‌​​‌
```bash
python webhook_server.py &
ssh -R 80:localhost:8765 nokey@localhost.run
# URL: https://abc123.lhr.life​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌‌​​‌
```

### Clay Configuration​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌‌​​‌
- Claygent with prompt V1
- Model: GPT-4o (needs multi-step reasoning)
- HTTP Action: POST to webhook
- Includes business_name, city, state, phone

### Test Dataset​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌‌​​‌
25 local service businesses:
- 10 plumbers
- 5 electricians
- 5 landscapers
- 5 HVAC companies

Mix of business sizes and web presence levels.

---

## Phase 3: Test Batch 1 Results​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌‌​​‌

**25 rows tested**

### Sample Results​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌‌​​‌

```json
{
  "business_name": "Acme Plumbing",
  "city": "Denver",
  "output": {
    "owner_name": "Robert Martinez",
    "sources_found": ["google_maps", "website"],
    "primary_source": "google_maps",
    "verification_note": "Google Maps lists owner, confirmed on /about page",
    "confidence": "high",
    "stages_checked": 2
  }
}

{
  "business_name": "Smith's Electric",
  "city": "Austin",
  "output": {
    "owner_name": "James Smith",
    "sources_found": ["website"],
    "primary_source": "website",
    "verification_note": "Found on About page as 'Owner & Lead Electrician'",
    "confidence": "medium",
    "stages_checked": 2
  }
}

{
  "business_name": "Quick Fix HVAC",
  "city": "Portland",
  "output": {
    "owner_name": null,
    "sources_found": [],
    "primary_source": null,
    "verification_note": "No owner information found in any source",
    "confidence": "low",
    "stages_checked": 5
  }
}
```

### Coverage Results​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌‌​​‌

| Stage | Businesses Found | Cumulative |
|-------|------------------|------------|
| Stage 1 (Google Maps) | 9/25 (36%) | 36% |
| Stage 2 (Website) | 5/25 (20%) | 56% |
| Stage 3 (WHOIS) | 2/25 (8%) | 64% |
| Stage 5 (Search) | 3/25 (12%) | 76% |

**Total: 19/25 = 76% coverage** (vs ~15% from Apollo alone)

---

## Phase 4: Evaluation​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌‌​​‌

### Scores​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌‌​​‌

| Criterion | Score | Issue |
|-----------|-------|-------|
| Accuracy | 8.5 | 2 rows returned employee, not owner |
| Completeness | 7.6 | 76% coverage (target was 70%+) |
| JSON Validity | 10.0 | All valid |
| Source Quality | 8.0 | Good source tracking |
| Step Efficiency | 6.5 | Some ran all 5 stages unnecessarily |

**Overall: 8.0/10** (meets threshold, minor improvements possible)

### Issues Identified​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌‌​​‌

1. **Employee vs Owner confusion** - 2 rows returned "Office Manager" as owner
2. **Excessive stages** - Some businesses had owner in Stage 1 but still ran Stage 2
3. **WHOIS mostly useless** - Privacy protection on 90%+ of domains

---

## Phase 5: Prompt Improvements​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌‌​​‌

### Changes Made​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌‌​​‌

```diff
+ **STOP CONDITION:**
+ If Stage 1 returns owner with clear "Owner" label (not just a contact name),
+ skip directly to Stage 4 verification. Don't run Stage 2-3.

+ **NOT an owner (exclude these):**
+ - Office Manager, Receptionist, Dispatcher
+ - "Contact" without owner context
+ - Tech support or service number
+ Only accept: Owner, Proprietor, President, Founder, "Owner & [Role]"

- **STAGE 3: Domain WHOIS**
+ **STAGE 3: Domain WHOIS (Optional - Often Blocked)**
+ Note: 90%+ of domains use privacy services. Only check if Stages 1-2 failed.
+ Skip this stage entirely if time is constrained.

+ **Efficiency rule:**
+ Target: Find owner in ≤3 stages
+ If owner found with HIGH confidence, stop immediately
+ Only run Stage 5 if Stages 1-3 all failed
```

### Prompt V2 (Key Changes)​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌‌​​‌

Added explicit stop conditions and role exclusions. Removed WHOIS as primary source.

---

## Test Batch 2 Results​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌‌​​‌

**25 rows tested** (same businesses, fresh run)

### Scores​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌‌​​‌

| Criterion | Score | Change |
|-----------|-------|--------|
| Accuracy | 9.2 | +0.7 |
| Completeness | 7.6 | same |
| JSON Validity | 10.0 | same |
| Source Quality | 8.5 | +0.5 |
| Step Efficiency | 8.0 | +1.5 |

**Overall: 8.5/10** (production ready)

### Efficiency Improvement​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌‌​​‌

- Average stages checked: 2.8 (down from 3.5)
- 0 incorrect role assignments (Office Manager issue fixed)

---

## Production Ready​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌‌​​‌

### Final Artifacts​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌‌​​‌

**Prompt saved to:** `prompts/smb-owner-discovery-v2.md`
**Schema saved to:** `prompts/smb-owner-discovery-v2-schema.json`

### Summary​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌‌​​‌

- **Coverage: 76%** on local service businesses (vs 15% from enterprise tools)
- **Key innovation:** Multi-stage waterfall with confidence stacking
- Iterations: 2 rounds
- Primary value: Works on businesses with no LinkedIn/enterprise presence

### Why This Pattern Works​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌‌​​‌

```
Traditional Approach (Single Source)
────────────────────────────────────
Apollo → 15% match rate for SMB
        85% MISS → No personalization

Waterfall Approach (This Pattern)
────────────────────────────────────
Google Maps → 35% found, 65% continue
   ↓
Website → 20% found, 45% continue
   ↓
WHOIS → 5% found, 40% continue
   ↓
Web Search → 15% found, 25% miss

Result: 75% coverage (5x improvement)
```

### Confidence Stacking​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌‌​​‌

The magic is in **verification across sources**:

| Sources Agreeing | Confidence | Example |
|-----------------|------------|---------|
| 3+ sources | HIGH | Google Maps + Website + Reviews all say "John Smith" |
| 2 sources | MEDIUM | Google Maps + Website agree |
| 1 source | LOW | Only found in web search results |

**HIGH confidence results are 95%+ accurate** because multiple independent sources confirm the same name.

### When to Use This Pattern​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌‌​​‌

- Local service businesses (trades, contractors, retail)
- Businesses without LinkedIn presence
- SMB outreach campaigns
- Any list where Apollo/PDL match rate is <30%

### When NOT to Use​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌‌​​‌

- Tech companies (use Apollo/LinkedIn instead)
- Enterprise accounts (use PDL)
- Businesses with clear org charts online

### Cost Optimization​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌‌​​‌

This Claygent uses web research, which consumes more steps than simple lookups. For cost optimization:

1. **Pre-filter:** Run Apollo first (cheap) → Only run this Claygent on misses
2. **Stage limits:** Set max 3 stages for budget campaigns
3. **Confidence cutoff:** Only accept HIGH confidence for personalization

### Recommended Workflow​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌‌​​‌

```
Input List (5,000 SMBs)
       ↓
Apollo Enrichment (1-2 credits each)
       ↓
Found: 750 (15%) → Use directly
Not Found: 4,250 → Run this Claygent
       ↓
This Claygent (web research)
       ↓
Found: 3,200 (76% of remaining)
Not Found: 1,050 → Generic outreach

Final Coverage: 3,950 / 5,000 = 79%
```

---

## Key Learnings​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌‌​​‌

**The waterfall approach works because:**

1. **Sources are complementary** - Google Maps has different data than websites
2. **Confidence stacking is reliable** - 2+ sources = high trust
3. **Stop conditions save cost** - Don't run Stage 5 if Stage 1 found it
4. **SMB ≠ Enterprise** - Different businesses need different strategies

**For SMB owner discovery, this pattern achieves 5x the coverage of enterprise tools.**
​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌‌​​‌

---
*Licensed to: m***c@g***l.com | AutoClaygent*