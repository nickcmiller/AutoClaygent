# Example: CRM Validation & Mismatch Detection​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌‌​​‌​​‌‌​​‌​​​‌‌​‌​‌

A complete worked example of building a Claygent to find where your existing CRM data is WRONG - not just add new data, but validate what you already have.

---

## User Request​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌‌​​‌​​‌‌​​‌​​​‌‌​‌​‌

"My CRM has 50,000 accounts with industry, company size, and business type fields. Sales says a lot of it is wrong. I need to validate what we have and flag mismatches so we can fix our segmentation."

---

## Pre-Phase: Native Integration Check​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌‌​​‌​​‌‌​​‌​​​‌‌​‌​‌

**First, check if Clay has native integrations:**

From `clay-datasets` skill:
- Clearbit, Apollo, PDL: Add new data, but don't compare to existing
- BuiltWith: Tech detection only

**The Problem:**
Native integrations *add* data. They don't *validate* your existing data against reality. If your CRM says "Enterprise" but Apollo also says "Enterprise," you haven't validated anything - you've just confirmed two databases agree (which may both be wrong).

**Real validation requires:** Checking the company's *actual website* to see if your CRM data matches reality.

**User chose Custom Claygent:** "I don't need more data - I need to know which of my 50,000 records are WRONG so I can fix them."

---

## Phase 0: Problem Understanding​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌‌​​‌​​‌‌​​‌​​​‌‌​‌​‌

### Why Validation Beats Enrichment​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌‌​​‌​​‌‌​​‌​​​‌‌​‌​‌

Most data workflows:
```
CRM Data → Enrich with Clearbit → Still might be wrong
           (Just added more fields)
```

Validation workflow:
```
CRM Data → Compare to actual website → Find mismatches → Fix wrong data
           (Actually validates accuracy)
```

**The insight:** Finding 5,000 records that are WRONG is more valuable than adding data to 50,000 records.

### Data Points to Validate​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌‌​​‌​​‌‌​​‌​​​‌‌​‌​‌

User wants to validate these CRM fields:
1. **Industry** - CRM says "Healthcare" - is that accurate?
2. **Company Size** - CRM says "Enterprise" - does website support that?
3. **Business Type** - CRM says "B2B" - does company actually serve businesses?

**Approach:** For each field, output match/mismatch/unknown with evidence.

### Input Data​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌‌​​‌​​‌‌​​‌​​​‌‌​‌​‌
- Domain (e.g., acme.com)
- CRM industry value (e.g., "Healthcare")
- CRM size value (e.g., "Enterprise")
- CRM business type (e.g., "B2B")

### Example Inputs​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌‌​​‌​​‌‌​​‌​​​‌‌​‌​‌
```
domain: acmesolutions.com
crm_industry: Healthcare
crm_size: Enterprise
crm_business_type: B2B
```

### Model Selected​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌‌​​‌​​‌‌​​‌​​​‌‌​‌​‌
GPT-4o (needs nuanced comparison logic)

---

## Phase 1: Prompt Drafting​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌‌​​‌​​‌‌​​‌​​​‌‌​‌​‌

### Prompt V1​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌‌​​‌​​‌‌​​‌​​​‌‌​‌​‌

```
**CRM Validation Task**

You have existing CRM data for a company. Your job is to CHECK if this data is CORRECT by visiting their actual website.

**Company:** {{domain}}

**CRM Data to Validate:**
- Industry: {{crm_industry}}
- Company Size: {{crm_size}}
- Business Type: {{crm_business_type}}

---

**VALIDATION STEPS:**

**Step 1: Visit the website**
Go to {{domain}} and check:
- Homepage
- About/Company page
- Footer/contact info

---

**Step 2: Validate INDUSTRY**

Check what industry the company ACTUALLY operates in.

Evidence to look for:
- What services/products do they offer?
- What industry keywords appear on the site?
- Who are their stated customers?

Then compare to CRM value: "{{crm_industry}}"

Output:
- match: Website clearly confirms this industry
- mismatch: Website shows a DIFFERENT industry (specify which)
- unknown: Couldn't determine industry from website

---

**Step 3: Validate COMPANY SIZE**

The CRM says: "{{crm_size}}"

Size signals to look for:
- "Enterprise": Multiple office locations, hundreds/thousands of employees, Fortune 500 language
- "Mid-Market": 50-500 employees, regional presence, B2B focus
- "SMB": Small team language, single location, local focus
- "Startup": Funded, growth-focused, lean team

Website signals:
- Team page (count members)
- Office locations (multiple = larger)
- Customer logos (enterprise customers = larger company)
- Language ("small team" vs "global presence")

Compare to CRM value and output match/mismatch/unknown.

---

**Step 4: Validate BUSINESS TYPE**

The CRM says: "{{crm_business_type}}"

B2B signals:
- Top navigation has "Enterprise" / "Business" / "For Teams"
- Pricing page shows per-seat or contract pricing
- Case studies feature other businesses
- No consumer/individual pricing

B2C signals:
- "Sign up free" / individual pricing
- Consumer-focused language
- App store links
- Social media shopping integration

Output match/mismatch/unknown.

---

**OUTPUT FORMAT:**

{
  "industry_validation": {
    "crm_value": "{{crm_industry}}",
    "website_value": "What you actually found" or null,
    "status": "match" | "mismatch" | "unknown",
    "evidence": "What you saw that determined this"
  },
  "size_validation": {
    "crm_value": "{{crm_size}}",
    "website_value": "What you actually found" or null,
    "status": "match" | "mismatch" | "unknown",
    "evidence": "What you saw that determined this"
  },
  "business_type_validation": {
    "crm_value": "{{crm_business_type}}",
    "website_value": "What you actually found" or null,
    "status": "match" | "mismatch" | "unknown",
    "evidence": "What you saw that determined this"
  },
  "overall_accuracy": "all_match" | "some_mismatch" | "all_mismatch" | "mostly_unknown",
  "priority_flag": "high_priority" | "review_needed" | "likely_accurate",
  "recommendation": "What action to take on this record"
}

**PRIORITY FLAGS:**
- high_priority: 2+ mismatches, or industry mismatch (affects segmentation)
- review_needed: 1 mismatch or mostly unknown
- likely_accurate: All match or unknown with 1+ match

**CRITICAL RULES:**
- DO NOT default to confirming the CRM. If you can't verify, say "unknown"
- A "match" requires POSITIVE evidence from the website
- "Unknown" is a valid answer - don't guess
- Be specific in evidence - cite what you actually saw
```

### JSON Schema V1​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌‌​​‌​​‌‌​​‌​​​‌‌​‌​‌

```json
{
  "type": "object",
  "properties": {
    "industry_validation": {
      "type": "object",
      "properties": {
        "crm_value": {"type": "string"},
        "website_value": {"anyOf": [{"type": "string"}, {"type": "null"}]},
        "status": {"type": "string", "enum": ["match", "mismatch", "unknown"]},
        "evidence": {"type": "string"}
      },
      "required": ["crm_value", "status", "evidence"]
    },
    "size_validation": {
      "type": "object",
      "properties": {
        "crm_value": {"type": "string"},
        "website_value": {"anyOf": [{"type": "string"}, {"type": "null"}]},
        "status": {"type": "string", "enum": ["match", "mismatch", "unknown"]},
        "evidence": {"type": "string"}
      },
      "required": ["crm_value", "status", "evidence"]
    },
    "business_type_validation": {
      "type": "object",
      "properties": {
        "crm_value": {"type": "string"},
        "website_value": {"anyOf": [{"type": "string"}, {"type": "null"}]},
        "status": {"type": "string", "enum": ["match", "mismatch", "unknown"]},
        "evidence": {"type": "string"}
      },
      "required": ["crm_value", "status", "evidence"]
    },
    "overall_accuracy": {
      "type": "string",
      "enum": ["all_match", "some_mismatch", "all_mismatch", "mostly_unknown"]
    },
    "priority_flag": {
      "type": "string",
      "enum": ["high_priority", "review_needed", "likely_accurate"]
    },
    "recommendation": {
      "type": "string",
      "description": "What action to take"
    }
  },
  "required": ["industry_validation", "size_validation", "business_type_validation", "overall_accuracy", "priority_flag", "recommendation"],
  "additionalProperties": false
}
```

---

## Phase 2: Testing Setup​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌‌​​‌​​‌‌​​‌​​​‌‌​‌​‌

### Webhook Server​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌‌​​‌​​‌‌​​‌​​​‌‌​‌​‌
```bash
python webhook_server.py &
ssh -R 80:localhost:8765 nokey@localhost.run
```

### Clay Configuration​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌‌​​‌​​‌‌​​‌​​​‌‌​‌​‌
- Claygent with prompt V1
- Model: GPT-4o
- Inputs: domain, crm_industry, crm_size, crm_business_type

### Test Dataset​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌‌​​‌​​‌‌​​‌​​​‌‌​‌​‌
30 accounts where sales suspected bad data:
- Mix of industries (Healthcare, Technology, Financial Services)
- Mix of sizes (SMB, Mid-Market, Enterprise)
- Mix of business types (B2B, B2C, Both)

---

## Phase 3: Test Batch 1 Results​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌‌​​‌​​‌‌​​‌​​​‌‌​‌​‌

**30 rows tested**

### Sample Results​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌‌​​‌​​‌‌​​‌​​​‌‌​‌​‌

```json
{
  "domain": "techstartup.io",
  "crm_industry": "Healthcare",
  "crm_size": "Enterprise",
  "crm_business_type": "B2B",
  "output": {
    "industry_validation": {
      "crm_value": "Healthcare",
      "website_value": "Technology/SaaS",
      "status": "mismatch",
      "evidence": "Website describes 'cloud infrastructure platform' with no healthcare mentions"
    },
    "size_validation": {
      "crm_value": "Enterprise",
      "website_value": "Startup",
      "status": "mismatch",
      "evidence": "About page shows 8-person team, founded 2022, Series A funding"
    },
    "business_type_validation": {
      "crm_value": "B2B",
      "website_value": "B2B",
      "status": "match",
      "evidence": "Enterprise pricing, per-seat model, business customer logos"
    },
    "overall_accuracy": "some_mismatch",
    "priority_flag": "high_priority",
    "recommendation": "Update industry to Technology/SaaS, update size to Startup. Business type is correct."
  }
}

{
  "domain": "localdental.com",
  "crm_industry": "Healthcare",
  "crm_size": "SMB",
  "crm_business_type": "B2C",
  "output": {
    "industry_validation": {
      "crm_value": "Healthcare",
      "website_value": "Healthcare/Dental",
      "status": "match",
      "evidence": "Dental practice, services include cleanings, implants, orthodontics"
    },
    "size_validation": {
      "crm_value": "SMB",
      "website_value": "SMB",
      "status": "match",
      "evidence": "Single location, team of 6 shown on About page"
    },
    "business_type_validation": {
      "crm_value": "B2C",
      "website_value": "B2C",
      "status": "match",
      "evidence": "Consumer dental services, online booking for individuals"
    },
    "overall_accuracy": "all_match",
    "priority_flag": "likely_accurate",
    "recommendation": "No updates needed. CRM data is accurate."
  }
}
```

### Mismatch Distribution​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌‌​​‌​​‌‌​​‌​​​‌‌​‌​‌

| Field | Mismatches | Match | Unknown |
|-------|------------|-------|---------|
| Industry | 8/30 (27%) | 19/30 | 3/30 |
| Company Size | 11/30 (37%) | 15/30 | 4/30 |
| Business Type | 4/30 (13%) | 24/30 | 2/30 |

**Key finding:** 37% had wrong company size - the most commonly inaccurate field.

---

## Phase 4: Evaluation​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌‌​​‌​​‌‌​​‌​​​‌‌​‌​‌

### Scores​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌‌​​‌​​‌‌​​‌​​​‌‌​‌​‌

| Criterion | Score | Issue |
|-----------|-------|-------|
| Accuracy | 8.5 | 2 rows incorrectly flagged mismatch |
| Completeness | 9.0 | Very few unknowns |
| JSON Validity | 10.0 | All valid |
| Source Quality | 9.0 | Good evidence citation |
| Step Efficiency | 7.5 | Avg 3 steps |

**Overall: 8.6/10** (production ready with minor tweaks)

### Issues Identified​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌‌​​‌​​‌‌​​‌​​​‌‌​‌​‌

1. **False mismatch on size** - 2 rows flagged "Enterprise" as wrong when it was borderline
2. **Industry too specific** - Sometimes returned "Healthcare/Dental" vs CRM's "Healthcare"

---

## Phase 5: Prompt Improvements​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌‌​​‌​​‌‌​​‌​​​‌‌​‌​‌

### Changes Made​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌‌​​‌​​‌‌​​‌​​​‌‌​‌​‌

```diff
+ **SIZE VALIDATION - Be conservative:**
+ - Only flag mismatch if CLEARLY wrong (e.g., CRM says Enterprise but team of 5)
+ - Borderline cases (could be either) → match
+ - "Unknown" is better than false mismatch

+ **INDUSTRY MATCHING - Allow sub-categories:**
+ - "Healthcare/Dental" MATCHES "Healthcare"
+ - "Technology/SaaS" MATCHES "Technology"
+ - Only mismatch if completely different vertical
+ - Example: "Healthcare" vs "Retail" = mismatch
+ - Example: "Healthcare" vs "Healthcare/Medical Devices" = match

+ **EVIDENCE QUALITY:**
+ - Every mismatch MUST cite specific text from the website
+ - If you can't cite specific evidence, use "unknown" not "mismatch"
```

---

## Test Batch 2 Results​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌‌​​‌​​‌‌​​‌​​​‌‌​‌​‌

**30 rows tested** (same accounts)

### Scores​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌‌​​‌​​‌‌​​‌​​​‌‌​‌​‌

| Criterion | Score | Change |
|-----------|-------|--------|
| Accuracy | 9.2 | +0.7 |
| Completeness | 9.0 | same |
| JSON Validity | 10.0 | same |
| Source Quality | 9.5 | +0.5 |
| Step Efficiency | 7.5 | same |

**Overall: 8.9/10** (production ready)

---

## Production Ready​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌‌​​‌​​‌‌​​‌​​​‌‌​‌​‌

### Final Artifacts​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌‌​​‌​​‌‌​​‌​​​‌‌​‌​‌

**Prompt saved to:** `prompts/crm-validation-v2.md`
**Schema saved to:** `prompts/crm-validation-v2-schema.json`

### Summary​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌‌​​‌​​‌‌​​‌​​​‌‌​‌​‌

- **Found 27% industry mismatches, 37% size mismatches** in "suspect" CRM data
- Key innovation: Validate against reality, not just add more data
- Priority flags enable triage (fix high_priority first)
- Evidence citation enables human review

### Why This Pattern Works​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌‌​​‌​​‌‌​​‌​​​‌‌​‌​‌

**Traditional enrichment:**
```
CRM: Healthcare       →  Clearbit: Healthcare  →  "Confirmed!" (but both might be wrong)
     (might be wrong)      (might be wrong)
```

**Validation enrichment:**
```
CRM: Healthcare       →  Website: "Cloud Infrastructure"  →  MISMATCH! Fix it.
                           (ground truth)
```

**The website is ground truth.** Databases can propagate errors. The company's own website shows what they actually do.

### High-Value Outputs​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌‌​​‌​​‌‌​​‌​​​‌‌​‌​‌

The most valuable output isn't the data - it's the **priority_flag**:

| Flag | Action | Example |
|------|--------|---------|
| high_priority | Fix immediately | Industry wrong = bad segmentation |
| review_needed | Human review | Size unclear, might matter |
| likely_accurate | Skip | Data looks correct |

### When to Use This Pattern​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌‌​​‌​​‌‌​​‌​​​‌‌​‌​‌

- Data hygiene / CRM cleanup projects
- Before major segmentation or scoring changes
- When sales says "the data is wrong"
- After merging databases (find conflicts)
- Before ABM campaigns (validate target accounts)

### When NOT to Use​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌‌​​‌​​‌‌​​‌​​​‌‌​‌​‌

- Initial enrichment (use native integrations first)
- When you don't have existing data to validate
- When website data doesn't reflect your fields (e.g., employee count)

### ROI Calculation​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌‌​​‌​​‌‌​​‌​​​‌‌​‌​‌

For 50,000 accounts:
- Running validation Claygent on all: ~$2,500 (at scale pricing)
- Finding 37% size mismatches = 18,500 wrong records
- If wrong segmentation costs $10/wrong-record in wasted sales time → $185,000 in savings

**Validation ROI: 74x** (validation cost vs fixing wrong data downstream)

### Recommended Workflow​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌‌​​‌​​‌‌​​‌​​​‌‌​‌​‌

```
Step 1: Identify "suspect" accounts
        - Sales feedback
        - Old data (>2 years)
        - Missing fields

Step 2: Run validation Claygent

Step 3: Filter by priority_flag = "high_priority"

Step 4: Human review + fix (or auto-update if confidence high)

Step 5: Re-segment / re-score with clean data
```

---

## Key Learnings​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌‌​​‌​​‌‌​​‌​​​‌‌​‌​‌

**Finding errors is more valuable than adding data:**

| Approach | What You Get | Actual Value |
|----------|--------------|--------------|
| Add more fields | More data (might be wrong) | Questionable |
| Validate existing | Know what's wrong | Fix real problems |

**Validation signals trust:**
- `match` = CRM is accurate, proceed with confidence
- `mismatch` = CRM is wrong, fix before using
- `unknown` = Need human review

**The priority_flag is the key output** - it tells ops teams where to focus limited review time.
​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​‌​​‌‌‌​​‌​​‌‌​​‌​​​‌‌​‌​‌

---
*Licensed to: m***c@g***l.com | AutoClaygent*