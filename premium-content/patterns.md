# Claygent Prompt Patterns​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌​​​​‌‌​​‌​​​‌‌​‌‌‌

Production-tested patterns for building Claygents that actually work. These patterns are extracted from real GTM engineering projects and represent what top-performing teams actually build.

---

## Pattern 1: Business Model Classification​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌​​​​‌‌​​‌​​​‌‌​‌‌‌

**Use case:** Classify service businesses by revenue model (subscription vs transactional vs hybrid)

**Why it's valuable:** Most CRM data about "business type" is wrong. Explicit decision trees with step-by-step logic beat generic "find pricing info" prompts.

```
Given the company domain: {{domain}}

Classify this business by their revenue/pricing model.

**Research Steps:**

1. Navigate to {{domain}} and look for:
   - /pricing, /plans, /membership, or /subscribe pages
   - Explicit dollar amounts with time periods ($/month, $/year)

2. Classify based on what you find:

   SUBSCRIPTION = Recurring membership model
   - Look for: "monthly", "annual", "per seat", "subscription"
   - Customer pays regularly (weekly/monthly/yearly)
   - Examples: "$99/month", "Starting at $49/user/month"

   TRANSACTIONAL = Per-use or one-time payment
   - Look for: "per project", "one-time", "pay as you go"
   - Customer pays for each service/product
   - Examples: "$500 per audit", "Quote-based pricing"

   HYBRID = Offers BOTH recurring AND one-time options
   - Has subscription tiers AND pay-per-use options
   - Examples: "Base plan $49/mo + $5 per additional user"

   FREEMIUM = Free tier with paid upgrades
   - Explicit free plan mentioned alongside paid tiers

   ENTERPRISE = Custom/contact sales pricing
   - No public pricing, "Contact us", "Request demo"

3. If no pricing page exists:
   - Check /about or homepage for business model clues
   - Look at how they describe their service delivery

**Output as JSON:**
{
  "business_model": "subscription" | "transactional" | "hybrid" | "freemium" | "enterprise" | null,
  "pricing_found": true | false,
  "pricing_details": "Brief description of what you found" | null,
  "evidence_url": "URL where you found pricing info",
  "confidence": "high" | "medium" | "low"
}

**Confidence Rules:**
- HIGH: Explicit pricing page with clear model
- MEDIUM: Model inferred from service description
- LOW: Guessing based on industry norms
- Return null if truly uncertain
```

**JSON Schema:**
```json
{
  "type": "object",
  "properties": {
    "business_model": {
      "anyOf": [
        {"type": "string", "enum": ["subscription", "transactional", "hybrid", "freemium", "enterprise"]},
        {"type": "null"}
      ]
    },
    "pricing_found": {"type": "boolean"},
    "pricing_details": {"anyOf": [{"type": "string"}, {"type": "null"}]},
    "evidence_url": {"anyOf": [{"type": "string", "format": "uri"}, {"type": "null"}]},
    "confidence": {"type": "string", "enum": ["high", "medium", "low"]}
  },
  "required": ["business_model", "pricing_found", "confidence"],
  "additionalProperties": false
}
```

---

## Pattern 2: Platform Detection via URL Patterns​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌​​​​‌‌​​‌​​​‌‌​‌‌‌

**Use case:** Detect what SaaS platforms/tools a company uses by analyzing their portal and login URLs

**Why it's valuable:** URL subdomain patterns are 95%+ accurate vs text scraping at 70%. Companies use vendor-hosted portals that reveal their tech stack.

```
Given the company domain: {{domain}}

Detect what platforms this company uses by analyzing their portal/login URLs.

**Research Steps:**

1. Look for customer-facing portals:
   - Check {{domain}}/login, {{domain}}/portal, {{domain}}/app
   - Look for "Client Login", "Patient Portal", "Customer Portal" links
   - Check the footer for portal links

2. When you find a portal link, analyze the URL pattern:

   **Subdomain Patterns (highest confidence):**
   - {company}.platformname.com → Uses Platform Name
   - app.{company}.com redirecting to vendor → Uses that vendor
   - portal.vendorname.com/{company} → Uses Vendor Name

   **Common Platform URL Patterns:**
   - *.salesforce.com → Salesforce
   - *.hubspot.com → HubSpot
   - *.zendesk.com → Zendesk
   - *.freshdesk.com → Freshdesk
   - *.shopify.com → Shopify
   - *.squarespace.com → Squarespace
   - *.wix.com → Wix
   - *.webflow.io → Webflow
   - *.calendly.com → Calendly
   - *.typeform.com → Typeform

3. Check for scheduling/booking tools:
   - Look for "Book Now", "Schedule", "Appointments"
   - These often reveal Calendly, Acuity, Cal.com, etc.

4. Check for chat widgets:
   - Intercom, Drift, Zendesk Chat, LiveChat
   - Usually visible in bottom-right corner

**Output as JSON:**
{
  "platforms_detected": [
    {
      "platform_name": "Name of platform",
      "platform_category": "CRM | Support | Scheduling | Chat | Website | E-commerce | Other",
      "detection_method": "subdomain | redirect | widget | integration_page",
      "evidence_url": "URL that revealed this"
    }
  ],
  "portal_url": "Customer portal URL if found" | null,
  "sources_checked": ["list of URLs visited"],
  "confidence": "high" | "medium" | "low"
}

**Confidence Rules:**
- HIGH: URL pattern match (subdomain or redirect)
- MEDIUM: Widget detected or mentioned on integrations page
- LOW: Inferred from job postings or indirect mentions
```

**JSON Schema:**
```json
{
  "type": "object",
  "properties": {
    "platforms_detected": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "platform_name": {"type": "string"},
          "platform_category": {"type": "string", "enum": ["CRM", "Support", "Scheduling", "Chat", "Website", "E-commerce", "Other"]},
          "detection_method": {"type": "string", "enum": ["subdomain", "redirect", "widget", "integration_page"]},
          "evidence_url": {"type": "string"}
        },
        "required": ["platform_name", "platform_category", "detection_method"]
      }
    },
    "portal_url": {"anyOf": [{"type": "string", "format": "uri"}, {"type": "null"}]},
    "sources_checked": {"type": "array", "items": {"type": "string"}},
    "confidence": {"type": "string", "enum": ["high", "medium", "low"]}
  },
  "required": ["platforms_detected", "sources_checked", "confidence"],
  "additionalProperties": false
}
```

---

## Pattern 3: CRM Validation & Mismatch Detection​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌​​​​‌‌​​‌​​​‌‌​‌‌‌

**Use case:** Find where your existing CRM data is WRONG by comparing it to website reality

**Why it's valuable:** Finding errors in existing data is more valuable than adding new fields. This pattern validates what you already have.

```
Given:
- Company domain: {{domain}}
- CRM field to validate: {{field_name}}
- Current CRM value: {{crm_value}}

Validate if the CRM value matches what's actually on the company's website.

**Research Steps:**

1. Navigate to {{domain}} and find information related to {{field_name}}

2. Look in these locations based on field type:
   - Company size/employees → /about, /team, /careers
   - Industry/vertical → Homepage, /about, meta description
   - Location/HQ → /contact, /about, footer
   - Business type → /pricing, /about, homepage
   - Founded year → /about, footer, press releases

3. Compare what you find to the CRM value:

   MATCH = Website confirms CRM value
   - The information aligns closely or exactly

   MISMATCH = Website contradicts CRM value
   - Clear disagreement between sources
   - Document both values and the evidence

   STALE = CRM value appears outdated
   - Website shows newer/updated information
   - CRM might have been accurate in the past

   UNVERIFIABLE = Cannot confirm or deny
   - Information not available on website
   - Do NOT default to "match" - say you can't verify

**Output as JSON:**
{
  "field_validated": "{{field_name}}",
  "crm_value": "{{crm_value}}",
  "website_value": "What you found on website" | null,
  "validation_status": "match" | "mismatch" | "stale" | "unverifiable",
  "evidence": "Specific text or URL proving your finding",
  "evidence_url": "URL where you found the information",
  "recommendation": "keep" | "update" | "review" | "cannot_determine",
  "confidence": "high" | "medium" | "low"
}

**Important Rules:**
- If you can't find the information, say "unverifiable" not "match"
- Quote the specific text that proves mismatch
- "Stale" means CRM was probably right before but is now outdated
```

**JSON Schema:**
```json
{
  "type": "object",
  "properties": {
    "field_validated": {"type": "string"},
    "crm_value": {"type": "string"},
    "website_value": {"anyOf": [{"type": "string"}, {"type": "null"}]},
    "validation_status": {"type": "string", "enum": ["match", "mismatch", "stale", "unverifiable"]},
    "evidence": {"type": "string"},
    "evidence_url": {"anyOf": [{"type": "string", "format": "uri"}, {"type": "null"}]},
    "recommendation": {"type": "string", "enum": ["keep", "update", "review", "cannot_determine"]},
    "confidence": {"type": "string", "enum": ["high", "medium", "low"]}
  },
  "required": ["field_validated", "crm_value", "validation_status", "recommendation", "confidence"],
  "additionalProperties": false
}
```

---

## Pattern 4: Digital Maturity Profiling​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌​​​​‌‌​​‌​​​‌‌​‌‌‌

**Use case:** Score prospects by online sophistication using signals you already have (no website visit needed)

**Why it's valuable:** Segment BEFORE expensive enrichment. Email domain alone tells you a lot.

```
Given:
- Business name: {{business_name}}
- Email address: {{email}} (optional)
- Phone number: {{phone}} (optional)
- Years in operation: {{years}} (optional)

Profile this business's digital maturity without visiting their website.

**Analysis Steps:**

1. Analyze email domain:

   PERSONAL_EMAIL = Gmail, Yahoo, Hotmail, Outlook, AOL, iCloud
   → Strong signal: Likely no business website
   → Segment: "Digital Dabbler" - needs basic online presence

   BUSINESS_EMAIL = Custom domain (john@acmeplumbing.com)
   → Has some web presence
   → Segment: "Digitally Active" - may need upgrades

   NO_EMAIL = Email field empty
   → Cannot determine from email

2. Analyze phone patterns:

   TOLL_FREE = 800/888/877/866 numbers
   → Suggests established business infrastructure

   LOCAL = Standard area code
   → Could be any size

   MOBILE = Identified mobile pattern
   → Often indicates smaller/solo operation

3. Consider years in operation (if available):

   0-2 years = "Early Stage" - still establishing
   2-5 years = "Growth Phase" - ready for optimization
   5+ years = "Established" - may have legacy systems

4. Generate overall profile:

**Output as JSON:**
{
  "email_type": "personal" | "business" | "none",
  "email_domain": "Domain extracted from email" | null,
  "phone_type": "toll_free" | "local" | "mobile" | "none",
  "maturity_segment": "digital_dabbler" | "digitally_active" | "digitally_mature" | "unknown",
  "maturity_signals": ["List of signals that informed this"],
  "recommended_approach": "Suggested sales angle based on maturity",
  "has_website_likely": true | false | null,
  "confidence": "high" | "medium" | "low"
}

**Segment Definitions:**
- digital_dabbler: Personal email, likely no website, needs foundational help
- digitally_active: Business email, has website, needs optimization
- digitally_mature: Professional email, toll-free, established presence
```

**JSON Schema:**
```json
{
  "type": "object",
  "properties": {
    "email_type": {"type": "string", "enum": ["personal", "business", "none"]},
    "email_domain": {"anyOf": [{"type": "string"}, {"type": "null"}]},
    "phone_type": {"anyOf": [{"type": "string", "enum": ["toll_free", "local", "mobile", "none"]}, {"type": "null"}]},
    "maturity_segment": {"type": "string", "enum": ["digital_dabbler", "digitally_active", "digitally_mature", "unknown"]},
    "maturity_signals": {"type": "array", "items": {"type": "string"}},
    "recommended_approach": {"type": "string"},
    "has_website_likely": {"anyOf": [{"type": "boolean"}, {"type": "null"}]},
    "confidence": {"type": "string", "enum": ["high", "medium", "low"]}
  },
  "required": ["email_type", "maturity_segment", "maturity_signals", "confidence"],
  "additionalProperties": false
}
```

---

## Pattern 5: B2B vs B2C Segment Detection​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌​​​​‌‌​​‌​​​‌‌​‌‌‌

**Use case:** Determine if a company primarily serves businesses or consumers

**Why it's valuable:** Navigation menu is the PRIMARY signal - companies intentionally put their target market in the nav.

```
Given the company domain: {{domain}}

Determine whether this company primarily serves B2B (businesses) or B2C (consumers).

**Research Steps:**

1. CHECK NAVIGATION FIRST (Primary Signal):
   - Look at the main navigation menu
   - B2B signals in nav: "Business", "Enterprise", "Employers", "Partners", "Solutions"
   - B2C signals in nav: "Shop", "Personal", "Individuals", "Families"
   - If nav contains B2B keywords → Strong B2B signal

2. Check pricing/plans page (Secondary Signal):
   - "Per seat", "Per user", "Team pricing" → B2B
   - "Per person", "Family plan", "Individual" → B2C
   - Enterprise tier with "Contact sales" → B2B

3. Check homepage messaging (Tertiary Signal):
   - "For teams", "For your business", "Enterprise-grade" → B2B
   - "For you", "Personal use", "Individuals" → B2C

4. Classification rules:

   B2B_ONLY = Exclusively serves businesses
   - Navigation explicitly B2B focused
   - No consumer-facing options

   B2C_ONLY = Exclusively serves consumers
   - No business/enterprise options
   - Individual/personal focus throughout

   BOTH = Serves both segments
   - Has separate B2B and B2C sections
   - Example: "Personal" and "Business" in nav

   UNKNOWN = Cannot determine
   - Ambiguous messaging
   - DO NOT default to one or the other

**Output as JSON:**
{
  "segment": "b2b_only" | "b2c_only" | "both" | "unknown",
  "primary_signal": "What you found in navigation",
  "secondary_signals": ["Other supporting evidence"],
  "b2b_indicators": ["List of B2B signals found"],
  "b2c_indicators": ["List of B2C signals found"],
  "evidence_url": "URL with strongest evidence",
  "confidence": "high" | "medium" | "low"
}

**CRITICAL RULE:**
Absence of B2B signals does NOT mean B2C.
Many companies don't explicitly state their target.
Use "unknown" when genuinely uncertain.
```

**JSON Schema:**
```json
{
  "type": "object",
  "properties": {
    "segment": {"type": "string", "enum": ["b2b_only", "b2c_only", "both", "unknown"]},
    "primary_signal": {"anyOf": [{"type": "string"}, {"type": "null"}]},
    "secondary_signals": {"type": "array", "items": {"type": "string"}},
    "b2b_indicators": {"type": "array", "items": {"type": "string"}},
    "b2c_indicators": {"type": "array", "items": {"type": "string"}},
    "evidence_url": {"anyOf": [{"type": "string", "format": "uri"}, {"type": "null"}]},
    "confidence": {"type": "string", "enum": ["high", "medium", "low"]}
  },
  "required": ["segment", "b2b_indicators", "b2c_indicators", "confidence"],
  "additionalProperties": false
}
```

---

## Pattern 6: Corporate Structure Detection​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌​​​​‌‌​​‌​​​‌‌​‌‌‌

**Use case:** Identify if this is a standalone company, subsidiary, or part of a larger organization

**Why it's valuable:** Selling to a subsidiary wastes time. Find the parent decision-maker.

```
Given the company domain: {{domain}}

Determine the corporate structure of this organization.

**Research Steps:**

1. Check footer and legal pages:
   - Look for "A [Parent Company] company"
   - Check "Terms of Service" for legal entity name
   - Look for copyright holder (different from brand name?)

2. Check /about page:
   - "Part of the [X] family of companies"
   - "A division of [X]"
   - "Acquired by [X] in [year]"
   - "Backed by [PE firm]" (suggests PE ownership)

3. Look for multi-location indicators:
   - /locations page with many addresses
   - "Find a location near you"
   - Franchise indicators

4. Classify the structure:

   INDEPENDENT = Standalone company
   - Single brand, single legal entity
   - Founder/CEO makes decisions
   - No parent company mentioned
   - Typically 1-50 locations

   SUBSIDIARY = Part of larger organization
   - Parent company mentioned
   - May share resources with siblings
   - Decision-maker is likely at parent level

   FRANCHISE = Individual owner, corporate rules
   - Franchise branding visible
   - Local owner but corporate policies
   - Mix of independence and constraints

   PE_BACKED = Private equity owned
   - "Portfolio company of [X]"
   - PE firm mentioned in about/news
   - May have holding company structure

   HOLDING_COMPANY = Owns multiple brands
   - This domain manages other companies
   - "Our brands" or "Our companies" section

**Output as JSON:**
{
  "structure_type": "independent" | "subsidiary" | "franchise" | "pe_backed" | "holding_company" | "unknown",
  "parent_company": "Parent company name if applicable" | null,
  "parent_domain": "Parent company domain" | null,
  "ownership_evidence": "Text that revealed the structure",
  "location_count": number | null,
  "decision_maker_level": "local" | "regional" | "parent" | "unknown",
  "evidence_url": "URL with structure information",
  "confidence": "high" | "medium" | "low"
}

**Sales Implications:**
- independent → Sell directly to leadership
- subsidiary → May need to engage parent
- franchise → Local decision with corporate approval
- pe_backed → Portfolio-level decisions possible
```

**JSON Schema:**
```json
{
  "type": "object",
  "properties": {
    "structure_type": {"type": "string", "enum": ["independent", "subsidiary", "franchise", "pe_backed", "holding_company", "unknown"]},
    "parent_company": {"anyOf": [{"type": "string"}, {"type": "null"}]},
    "parent_domain": {"anyOf": [{"type": "string"}, {"type": "null"}]},
    "ownership_evidence": {"anyOf": [{"type": "string"}, {"type": "null"}]},
    "location_count": {"anyOf": [{"type": "integer"}, {"type": "null"}]},
    "decision_maker_level": {"type": "string", "enum": ["local", "regional", "parent", "unknown"]},
    "evidence_url": {"anyOf": [{"type": "string", "format": "uri"}, {"type": "null"}]},
    "confidence": {"type": "string", "enum": ["high", "medium", "low"]}
  },
  "required": ["structure_type", "decision_maker_level", "confidence"],
  "additionalProperties": false
}
```

---

## Pattern 7: Buying Intent Detection​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌​​​​‌‌​​‌​​​‌‌​‌‌‌

**Use case:** Find companies ready to buy NOW based on timing signals

**Why it's valuable:** "Company exists" means nothing. "Company raised $5M 2 months ago" = actively buying.

```
Given the company: {{company_name}} ({{domain}})

Research recent signals that indicate buying intent or timing.

**Research Steps:**

1. Search for recent funding:
   - Search: "{{company_name}}" funding OR raised OR series
   - Check Crunchbase, TechCrunch, press releases
   - Recent funding (< 6 months) = 90-day buying window

2. Search for leadership changes:
   - Search: "{{company_name}}" hired OR appointed OR joins
   - New VP+ executives often evaluate new vendors
   - New CEO/CFO = major vendor review likely

3. Check for expansion signals:
   - Search: "{{company_name}}" expanding OR growth OR hiring
   - New office locations
   - Significant hiring (10+ open roles)

4. Check job postings:
   - Search: site:linkedin.com/jobs "{{company_name}}"
   - Hiring for specific tools (looking for "Salesforce admin" = uses Salesforce)
   - Many open roles = budget available

5. Score the buying window:

**Output as JSON:**
{
  "buying_signals": [
    {
      "signal_type": "funding" | "leadership_change" | "expansion" | "hiring" | "news",
      "signal_detail": "Description of what you found",
      "recency": "< 1 month" | "1-3 months" | "3-6 months" | "6-12 months" | "> 12 months",
      "source_url": "URL of the source"
    }
  ],
  "buying_window_score": 0-100,
  "timing_assessment": "hot" | "warm" | "neutral" | "cold",
  "recommended_action": "What to do based on timing",
  "last_significant_event": "Most recent relevant event" | null,
  "last_event_date": "YYYY-MM" | null,
  "confidence": "high" | "medium" | "low"
}

**Scoring Guide:**
- 80-100 (HOT): Funding < 3 months OR new exec < 2 months
- 60-79 (WARM): Funding 3-6 months OR expansion news
- 40-59 (NEUTRAL): No recent signals but active company
- 0-39 (COLD): No activity, possible concerns
```

**JSON Schema:**
```json
{
  "type": "object",
  "properties": {
    "buying_signals": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "signal_type": {"type": "string", "enum": ["funding", "leadership_change", "expansion", "hiring", "news"]},
          "signal_detail": {"type": "string"},
          "recency": {"type": "string", "enum": ["< 1 month", "1-3 months", "3-6 months", "6-12 months", "> 12 months"]},
          "source_url": {"type": "string"}
        },
        "required": ["signal_type", "signal_detail", "recency"]
      }
    },
    "buying_window_score": {"type": "integer", "minimum": 0, "maximum": 100},
    "timing_assessment": {"type": "string", "enum": ["hot", "warm", "neutral", "cold"]},
    "recommended_action": {"type": "string"},
    "last_significant_event": {"anyOf": [{"type": "string"}, {"type": "null"}]},
    "last_event_date": {"anyOf": [{"type": "string"}, {"type": "null"}]},
    "confidence": {"type": "string", "enum": ["high", "medium", "low"]}
  },
  "required": ["buying_signals", "buying_window_score", "timing_assessment", "confidence"],
  "additionalProperties": false
}
```

---

## Pattern 8: Multi-Axis Classification Engine​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌​​​​‌‌​​‌​​​‌‌​‌‌‌

**Use case:** Classify a company along multiple independent dimensions simultaneously

**Why it's valuable:** A company can be "SMB" on size but "Enterprise" on tech stack. Single-dimension classification misses nuance.

```
Given the company domain: {{domain}}

Classify this company across multiple dimensions. Each dimension is independent.

**Dimensions to Classify:**

1. ORGANIZATION SIZE
   - solo: 1 person / freelancer
   - small: 2-10 employees
   - medium: 11-50 employees
   - large: 51-200 employees
   - enterprise: 200+ employees

2. BUSINESS MODEL
   - subscription: Recurring revenue
   - transactional: Per-transaction fees
   - hybrid: Mix of both
   - enterprise_sales: Custom contracts

3. GEOGRAPHIC SCOPE
   - local: Single city/metro area
   - regional: Multi-city or state
   - national: Country-wide
   - international: Multiple countries

4. TECH SOPHISTICATION
   - basic: Minimal tech presence
   - moderate: Standard business tools
   - advanced: Modern SaaS stack
   - enterprise: Complex integrations

5. MARKET POSITION
   - emerging: New/early stage
   - growing: Expanding presence
   - established: Stable market position
   - dominant: Market leader

**Research approach:**
- Visit /about, /team, /careers for size signals
- Check /pricing for business model
- Look at /locations or footer for geography
- Analyze website tech for sophistication

**Output as JSON:**
{
  "dimensions": {
    "organization_size": {
      "value": "solo" | "small" | "medium" | "large" | "enterprise" | null,
      "confidence": 0-100,
      "evidence": "What you found"
    },
    "business_model": {
      "value": "subscription" | "transactional" | "hybrid" | "enterprise_sales" | null,
      "confidence": 0-100,
      "evidence": "What you found"
    },
    "geographic_scope": {
      "value": "local" | "regional" | "national" | "international" | null,
      "confidence": 0-100,
      "evidence": "What you found"
    },
    "tech_sophistication": {
      "value": "basic" | "moderate" | "advanced" | "enterprise" | null,
      "confidence": 0-100,
      "evidence": "What you found"
    },
    "market_position": {
      "value": "emerging" | "growing" | "established" | "dominant" | null,
      "confidence": 0-100,
      "evidence": "What you found"
    }
  },
  "sources_checked": ["URLs visited"],
  "overall_confidence": "high" | "medium" | "low"
}

**Rules:**
- Each dimension is scored independently
- Return null for dimensions you can't determine
- Confidence is per-dimension (0-100)
```

**JSON Schema:**
```json
{
  "type": "object",
  "properties": {
    "dimensions": {
      "type": "object",
      "properties": {
        "organization_size": {
          "type": "object",
          "properties": {
            "value": {"anyOf": [{"type": "string", "enum": ["solo", "small", "medium", "large", "enterprise"]}, {"type": "null"}]},
            "confidence": {"type": "integer", "minimum": 0, "maximum": 100},
            "evidence": {"type": "string"}
          },
          "required": ["value", "confidence"]
        },
        "business_model": {
          "type": "object",
          "properties": {
            "value": {"anyOf": [{"type": "string", "enum": ["subscription", "transactional", "hybrid", "enterprise_sales"]}, {"type": "null"}]},
            "confidence": {"type": "integer", "minimum": 0, "maximum": 100},
            "evidence": {"type": "string"}
          },
          "required": ["value", "confidence"]
        },
        "geographic_scope": {
          "type": "object",
          "properties": {
            "value": {"anyOf": [{"type": "string", "enum": ["local", "regional", "national", "international"]}, {"type": "null"}]},
            "confidence": {"type": "integer", "minimum": 0, "maximum": 100},
            "evidence": {"type": "string"}
          },
          "required": ["value", "confidence"]
        },
        "tech_sophistication": {
          "type": "object",
          "properties": {
            "value": {"anyOf": [{"type": "string", "enum": ["basic", "moderate", "advanced", "enterprise"]}, {"type": "null"}]},
            "confidence": {"type": "integer", "minimum": 0, "maximum": 100},
            "evidence": {"type": "string"}
          },
          "required": ["value", "confidence"]
        },
        "market_position": {
          "type": "object",
          "properties": {
            "value": {"anyOf": [{"type": "string", "enum": ["emerging", "growing", "established", "dominant"]}, {"type": "null"}]},
            "confidence": {"type": "integer", "minimum": 0, "maximum": 100},
            "evidence": {"type": "string"}
          },
          "required": ["value", "confidence"]
        }
      },
      "required": ["organization_size", "business_model", "geographic_scope", "tech_sophistication", "market_position"]
    },
    "sources_checked": {"type": "array", "items": {"type": "string"}},
    "overall_confidence": {"type": "string", "enum": ["high", "medium", "low"]}
  },
  "required": ["dimensions", "sources_checked", "overall_confidence"],
  "additionalProperties": false
}
```

---

## Pattern 9: SMB Owner Discovery Pipeline​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌​​​​‌‌​​‌​​​‌‌​‌‌‌

**Use case:** Find the owner of a small business using a multi-stage waterfall approach

**Why it's valuable:** No single source covers SMB. Waterfall approach with confidence stacking significantly improves coverage.

```
Given:
- Business name: {{business_name}}
- Business domain: {{domain}} (optional)
- Business address: {{address}} (optional)

Find the owner or primary decision-maker of this small business.

**Stage 1: Google Maps / Business Listing**
- Search Google Maps for "{{business_name}}" near "{{address}}"
- Look for owner name in the business listing
- Check if "Owner" is listed as a contact

**Stage 2: Website Team/About Page**
- Visit {{domain}}/about, {{domain}}/team, {{domain}}/our-story
- Look for "Owner", "Founder", "President", "Principal"
- Check for names with photos and leadership titles

**Stage 3: Website Contact/Footer**
- Check {{domain}}/contact for owner mentions
- Look at footer for "Owner operated" or founder mentions
- Check copyright for individual name (vs company name)

**Stage 4: Social Media Links**
- Find company LinkedIn, Facebook, Instagram from website
- Check LinkedIn company page for founder/owner
- Check Facebook "About" for owner name
- Look for "Owner at [Business]" in profiles

**Stage 5: Web Search Fallback**
- Search: "{{business_name}}" owner OR founder
- Search: "{{business_name}}" "{{city}}" owner
- Look for news articles, press, or local business features

**Confidence Stacking:**
- 1 source only = LOW confidence (50%)
- 2 sources agree = MEDIUM confidence (75%)
- 3+ sources agree = HIGH confidence (90%+)

**Output as JSON:**
{
  "owner_found": true | false,
  "owner_name": "Full name" | null,
  "owner_title": "Owner" | "Founder" | "President" | "Principal" | null,
  "sources": [
    {
      "source_type": "google_maps" | "website_about" | "website_contact" | "linkedin" | "facebook" | "web_search",
      "name_found": "Name from this source",
      "source_url": "URL"
    }
  ],
  "sources_agreeing": number,
  "linkedin_url": "LinkedIn profile URL" | null,
  "email": "Email if found" | null,
  "phone": "Phone if found" | null,
  "confidence": "high" | "medium" | "low",
  "confidence_score": 0-100
}

**Important Rules:**
- NEVER guess or make up names
- If sources disagree, note the conflict
- "Owner" > "Founder" > "President" for SMB relevance
- Return null if no owner can be found
```

**JSON Schema:**
```json
{
  "type": "object",
  "properties": {
    "owner_found": {"type": "boolean"},
    "owner_name": {"anyOf": [{"type": "string"}, {"type": "null"}]},
    "owner_title": {"anyOf": [{"type": "string", "enum": ["Owner", "Founder", "President", "Principal", "CEO", "Managing Partner"]}, {"type": "null"}]},
    "sources": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "source_type": {"type": "string", "enum": ["google_maps", "website_about", "website_contact", "linkedin", "facebook", "web_search", "other"]},
          "name_found": {"type": "string"},
          "source_url": {"type": "string"}
        },
        "required": ["source_type", "name_found"]
      }
    },
    "sources_agreeing": {"type": "integer"},
    "linkedin_url": {"anyOf": [{"type": "string", "format": "uri"}, {"type": "null"}]},
    "email": {"anyOf": [{"type": "string", "format": "email"}, {"type": "null"}]},
    "phone": {"anyOf": [{"type": "string"}, {"type": "null"}]},
    "confidence": {"type": "string", "enum": ["high", "medium", "low"]},
    "confidence_score": {"type": "integer", "minimum": 0, "maximum": 100}
  },
  "required": ["owner_found", "sources", "confidence", "confidence_score"],
  "additionalProperties": false
}
```

---

## Pattern Combinations​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌​​​​‌‌​​‌​​​‌‌​‌‌‌

These patterns can be combined for comprehensive enrichment:

### Combo A: Full Company Profile​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌​​​​‌‌​​‌​​​‌‌​‌‌‌
1. Pattern 1 (Business Model) → Revenue type
2. Pattern 5 (B2B/B2C Segment) → Target market
3. Pattern 6 (Corporate Structure) → Decision-maker level
4. Pattern 8 (Multi-Axis) → Detailed classification

### Combo B: Sales Timing Optimization​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌​​​​‌‌​​‌​​​‌‌​‌‌‌
1. Pattern 7 (Buying Intent) → Is now a good time?
2. Pattern 3 (CRM Validation) → Is our data still accurate?
3. Pattern 6 (Corporate Structure) → Who actually decides?

### Combo C: SMB Outreach​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌​​​​‌‌​​‌​​​‌‌​‌‌‌
1. Pattern 4 (Digital Maturity) → Segment before enrichment
2. Pattern 9 (Owner Discovery) → Find the decision-maker
3. Pattern 1 (Business Model) → Understand their business

### Combo D: Tech Stack Intelligence​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌​​​​‌‌​​‌​​​‌‌​‌‌‌
1. Pattern 2 (Platform Detection) → What do they use?
2. Pattern 8 (Multi-Axis) → Tech sophistication level
3. Pattern 7 (Buying Intent) → Are they likely to switch?

---

## Key Principles​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌​​​​‌‌​​‌​​​‌‌​‌‌‌

These patterns work because they follow these principles:

1. **URL patterns beat text parsing** - 95% accuracy vs 70%
2. **Navigation signals come first** - Check menu before body content
3. **Null is a feature** - Returning null when unsure beats wrong data
4. **Confidence by dimension** - Not one overall score
5. **Mismatch detection is valuable** - Finding errors in existing data
6. **Decision trees beat "find X"** - Explicit branching logic
7. **Evidence extraction required** - Always cite your sources
8. **Canonical values enforce consistency** - Use enums, not free text
​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌​​​​‌‌​​‌​​​‌‌​‌‌‌

---
*Licensed to: m***c@g***l.com | AutoClaygent*