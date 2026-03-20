# Example: Platform Detection via URL Patterns​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​​​​‌‌​​​​​​‌‌​‌‌‌​​‌‌​‌‌​

A complete worked example of detecting what SaaS platforms a company uses by analyzing their portal and login URLs.

---

## User Request​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​​​​‌‌​​​​​​‌‌​‌‌‌​​‌‌​‌‌​

"I want to identify what scheduling/booking platform a service business uses, given their domain. We're selling a competing product and need to find companies using specific vendors."

---

## Pre-Phase: Native Integration Check​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​​​​‌‌​​​​​​‌‌​‌‌‌​​‌‌​‌‌​

**First, check if Clay has native integrations:**

From `clay-datasets` skill:
- BuiltWith: 5-10 credits, detects technologies from page scripts
- Wappalyzer: Similar approach (JavaScript-based detection)

**The Problem with Text Scraping:**
BuiltWith and similar tools work by scanning HTML/JavaScript for vendor fingerprints. But for hosted SaaS platforms (scheduling, booking, CRM portals), the vendor's code often isn't on the main website at all - it's on a subdomain.

**User chose Custom Claygent:** "BuiltWith misses most of these. The booking systems are on subdomains like `book.company.com` or `company.vendorplatform.com` - I need URL pattern detection."

---

## Phase 0: Problem Understanding​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​​​​‌‌​​​​​​‌‌​‌‌‌​​‌‌​‌‌​

### Why URL Patterns Beat Text Scraping​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​​​​‌‌​​​​​​‌‌​‌‌‌​​‌‌​‌‌​

When a business uses a hosted SaaS platform, they get a portal URL in one of these formats:

```
Pattern A: {company}.vendorplatform.com     → Company hosted ON vendor's domain
Pattern B: book.{company}.com → CNAME       → Check where it redirects
Pattern C: {company}.com/schedule           → Embedded widget (harder to detect)
```

**Pattern A is 95%+ accurate** because:
- The URL subdomain IS the vendor - no scraping needed
- Companies can't fake this - DNS doesn't lie
- Works even if the main website never mentions the vendor

**Text scraping is ~70% accurate** because:
- Many platforms don't leave JavaScript fingerprints
- Main website may not mention the booking tool
- Enterprise sites often strip vendor branding

### Data Points Needed​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​​​​‌‌​​​​​​‌‌​‌‌‌​​‌‌​‌‌​
- Platform vendor name
- Portal URL pattern found
- Detection method used
- Confidence level

**Count: 4 data points = within limit**

### Input Data​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​​​​‌‌​​​​​​‌‌​‌‌‌​​‌‌​‌‌​
- Company domain (e.g., acmeplumbing.com)
- Optional: Known portal subdomain to check

### Example Inputs Provided​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​​​​‌‌​​​​​​‌‌​‌‌‌​​‌‌​‌‌​
```
acmeplumbing.com
downtowndental.com
cityautorepair.com
luxuryspa.com
```

### Target Platforms to Detect​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​​​​‌‌​​​​​​‌‌​‌‌‌​​‌‌​‌‌​
```
Platform A → {company}.platforma.com
Platform B → schedule.{company}.com (CNAME → platformb.io)
Platform C → {company}.platformc.io
Platform D → book.{company}.com (iframe from platformd.com)
```
*(Generic platform names - replace with actual vendors you're researching)*

### Model Selected​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​​​​‌‌​​​​​​‌‌​‌‌‌​​‌‌​‌‌​
GPT-4o (needs reliable URL resolution and pattern matching)

---

## Phase 1: Prompt Drafting​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​​​​‌‌​​​​​​‌‌​‌‌‌​​‌‌​‌‌​

### Prompt V1​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​​​​‌‌​​​​​​‌‌​‌‌‌​​‌‌​‌‌​

```
Given the service business domain: {{domain}}

Detect what scheduling/booking platform this business uses by analyzing URL patterns.

**Research steps:**

1. Check common portal URL patterns (most reliable):
   - {{domain_without_tld}}.platforma.com
   - {{domain_without_tld}}.platformb.io
   - {{domain_without_tld}}.platformc.com
   - schedule.{{domain}}
   - book.{{domain}}
   - booking.{{domain}}

   For each URL, check if it resolves (200 OK) and note what platform it points to.

2. If portal URL found, verify by checking:
   - Does the page have platform branding?
   - Does the SSL certificate show the vendor name?
   - What does the page title/meta say?

3. If no portal URL pattern works, check the main website:
   - Look for "Book Now" or "Schedule" buttons
   - Follow the link - where does it go?
   - Check for iframe embeds from known platforms

4. Fallback: Search for platform evidence
   - site:{{domain}} "powered by Platform A"
   - Check /book, /schedule, /appointments pages

**URL Pattern Confidence Rules:**
- HIGH: Direct subdomain match (company.platform.com)
- HIGH: CNAME redirect to known platform
- MEDIUM: Embedded widget/iframe detected
- LOW: Only text mention or "powered by" footer

**Canonical Platform Values:**
- "Platform A" (portal pattern: {company}.platforma.com)
- "Platform B" (portal pattern: {company}.platformb.io)
- "Platform C" (portal pattern: schedule.{company}.com CNAME)
- "Platform D" (embedded widget)
- "Other" (specify in notes)
- null (no platform detected)

**Output as JSON:**
{
  "platform_detected": "Platform A" | "Platform B" | "Platform C" | "Platform D" | "Other" | null,
  "portal_url": "The URL where we detected the platform" or null,
  "detection_method": "subdomain_match" | "cname_redirect" | "iframe_embed" | "text_mention" | "booking_button",
  "evidence": "What we found that confirms this",
  "confidence": "high" | "medium" | "low"
}

Return null for platform_detected if no scheduling platform can be confirmed.
Never guess based on industry - only report verified findings.
```

### JSON Schema V1​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​​​​‌‌​​​​​​‌‌​‌‌‌​​‌‌​‌‌​

```json
{
  "type": "object",
  "properties": {
    "platform_detected": {
      "anyOf": [
        {"type": "string", "enum": ["Platform A", "Platform B", "Platform C", "Platform D", "Other"]},
        {"type": "null"}
      ],
      "description": "The scheduling platform detected"
    },
    "portal_url": {
      "anyOf": [{"type": "string"}, {"type": "null"}],
      "description": "URL where platform was detected"
    },
    "detection_method": {
      "type": "string",
      "enum": ["subdomain_match", "cname_redirect", "iframe_embed", "text_mention", "booking_button"],
      "description": "How we detected the platform"
    },
    "evidence": {
      "anyOf": [{"type": "string"}, {"type": "null"}],
      "description": "Evidence confirming the detection"
    },
    "confidence": {
      "type": "string",
      "enum": ["high", "medium", "low"],
      "description": "Confidence in the detection"
    }
  },
  "required": ["platform_detected", "detection_method", "confidence"],
  "additionalProperties": false
}
```

---

## Phase 2: Testing Setup​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​​​​‌‌​​​​​​‌‌​‌‌‌​​‌‌​‌‌​

### Webhook Server​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​​​​‌‌​​​​​​‌‌​‌‌‌​​‌‌​‌‌​
```bash
python webhook_server.py &
ssh -R 80:localhost:8765 nokey@localhost.run
# URL: https://abc123.lhr.life​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​​​​‌‌​​​​​​‌‌​‌‌‌​​‌‌​‌‌​
```

### Clay Configuration​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​​​​‌‌​​​​​​‌‌​‌‌‌​​‌‌​‌‌​
- Created Claygent column with prompt V1
- Model: GPT-4o (BYOK)
- HTTP Action: POST to webhook
- Body includes row_id, domain, output, json_trace

---

## Phase 3: Test Batch 1 Results​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​​​​‌‌​​​​​​‌‌​‌‌‌​​‌‌​‌‌​

**25 rows tested** (service businesses across plumbing, dental, auto repair, spa)

Sample results:

```json
{
  "domain": "acmeplumbing.com",
  "output": {
    "platform_detected": "Platform A",
    "portal_url": "https://acmeplumbing.platforma.com",
    "detection_method": "subdomain_match",
    "evidence": "Direct subdomain resolves, shows Platform A branding",
    "confidence": "high"
  },
  "steps": 2
}

{
  "domain": "downtowndental.com",
  "output": {
    "platform_detected": "Platform B",
    "portal_url": "https://schedule.downtowndental.com",
    "detection_method": "cname_redirect",
    "evidence": "schedule.downtowndental.com CNAMEs to platformb.io",
    "confidence": "high"
  },
  "steps": 3
}

{
  "domain": "momandpopshop.com",
  "output": {
    "platform_detected": null,
    "portal_url": null,
    "detection_method": "subdomain_match",
    "evidence": "No portal URLs resolved, no booking button on site",
    "confidence": "high"
  },
  "steps": 4
}
```

---

## Phase 4: Evaluation​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​​​​‌‌​​​​​​‌‌​‌‌‌​​‌‌​‌‌​

### Scores​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​​​​‌‌​​​​​​‌‌​‌‌‌​​‌‌​‌‌​

| Criterion | Score | Issue |
|-----------|-------|-------|
| Accuracy | 9.2 | 2 rows had wrong platform (similar names) |
| Completeness | 8.5 | Found platform for 85% of businesses that have one |
| JSON Validity | 10.0 | All valid JSON |
| Source Quality | 9.5 | URL patterns are definitive evidence |
| Step Efficiency | 8.0 | Avg 3.1 steps |

**Overall: 9.0/10** (excellent - URL patterns work)

### Issues Identified​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​​​​‌‌​​​​​​‌‌​‌‌‌​​‌‌​‌‌​

1. **Platform name confusion** - 2 rows mixed up similarly-named platforms
2. **CNAME detection inconsistent** - Sometimes missed redirects
3. **Embedded widgets harder** - iframe detection less reliable

---

## Phase 5: Minor Improvements​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​​​​‌‌​​​​​​‌‌​‌‌‌​​‌‌​‌‌​

### Changes Made​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​​​​‌‌​​​​​​‌‌​‌‌‌​​‌‌​‌‌​

```diff
+ **Important:** Platform A and Platform AA are different vendors:
+   - Platform A: {company}.platforma.com (consumer-focused)
+   - Platform AA: {company}.platformaa.io (enterprise-focused)
+   Check the exact domain suffix carefully.

+ For CNAME detection:
+   - If schedule.{domain} returns 200 but looks different, check SSL cert
+   - The SSL certificate Common Name reveals the actual platform

+ For embedded widgets:
+   - Check page source for iframe src containing platform domains
+   - Look for script tags loading from platform CDNs
```

### Prompt V2 (excerpt of changes)​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​​​​‌‌​​​​​​‌‌​‌‌‌​​‌‌​‌‌​

Added platform disambiguation rules and improved CNAME detection instructions.

---

## Test Batch 2 Results​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​​​​‌‌​​​​​​‌‌​‌‌‌​​‌‌​‌‌​

**25 rows tested**

### Scores​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​​​​‌‌​​​​​​‌‌​‌‌‌​​‌‌​‌‌​

| Criterion | Score | Change |
|-----------|-------|--------|
| Accuracy | 9.6 | +0.4 |
| Completeness | 8.5 | same |
| JSON Validity | 10.0 | same |
| Source Quality | 9.5 | same |
| Step Efficiency | 7.5 | -0.5 (more thorough CNAME checks) |

**Overall: 9.2/10** (production ready)

---

## Production Ready​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​​​​‌‌​​​​​​‌‌​‌‌‌​​‌‌​‌‌​

### Final Artifacts​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​​​​‌‌​​​​​​‌‌​‌‌‌​​‌‌​‌‌​

**Prompt saved to:** `prompts/platform-detection-v2.md`
**Schema saved to:** `prompts/platform-detection-v2-schema.json`

### Summary​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​​​​‌‌​​​​​​‌‌​‌‌‌​​‌‌​‌‌​

- Iterations: 2 rounds to reach 9.0+
- Key insight: **URL subdomain patterns are 95%+ accurate** vs text scraping (~70%)
- Detection methods ranked by reliability:
  1. Direct subdomain match (most reliable)
  2. CNAME redirect analysis
  3. SSL certificate inspection
  4. Iframe/widget detection
  5. Text mention (least reliable)

### Why This Pattern Works​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​​​​‌‌​​​​​​‌‌​‌‌‌​​‌‌​‌‌​

Traditional tech detection (BuiltWith, Wappalyzer) scans JavaScript on the main website. But:

1. **SaaS portals live on subdomains** - The booking system isn't on the main site
2. **URLs are structural, not textual** - `company.platform.com` IS the platform
3. **DNS doesn't lie** - Companies can't fake subdomain ownership
4. **No false positives** - Either the URL resolves or it doesn't

### When to Use This Pattern​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​​​​‌‌​​​​​​‌‌​‌‌‌​​‌‌​‌‌​

- Detecting hosted SaaS platforms (scheduling, booking, portals)
- Competitive intelligence (finding users of specific vendors)
- Platform migration campaigns (targeting users of older systems)
- Any case where the product lives on a subdomain, not the main site

### When NOT to Use This Pattern​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​​​​‌‌​​​​​​‌‌​‌‌‌​​‌‌​‌‌​

- Self-hosted software (no predictable URL patterns)
- JavaScript-only integrations (analytics, chat widgets)
- Backend systems (CRMs, databases) with no public portal

### Extending the Pattern​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​​​​‌‌​​​​​​‌‌​‌‌‌​​‌‌​‌‌​

To adapt for your target platforms:

1. Research your competitors' URL patterns:
   - Sign up for free trials
   - Note the portal URL format
   - Document CNAME patterns

2. Build your URL pattern list:
   ```
   Platform X: {company}.platformx.com
   Platform Y: portal.{company}.com → platformy.io
   Platform Z: {company}-booking.platformz.net
   ```

3. Update the prompt with your specific patterns
4. Test on known customers of each platform to validate detection

---

## Key Learnings​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​​​​‌‌​​​​​​‌‌​‌‌‌​​‌‌​‌‌​

**URL pattern detection achieves 95%+ accuracy because:**

| Detection Method | Accuracy | Why |
|-----------------|----------|-----|
| Subdomain match | 95%+ | DNS is authoritative |
| CNAME redirect | 90%+ | Redirect destination is the platform |
| SSL certificate | 85%+ | Certs reveal actual host |
| Iframe embed | 75% | Can be obscured |
| Text mention | 70% | Often outdated or marketing-only |

**This pattern is more reliable than BuiltWith** for hosted platforms because it detects the actual platform URL, not JavaScript fingerprints that may or may not be present.
​​‌‌​‌​​​​‌‌​‌​‌​​‌‌​‌​​​​‌‌​‌‌‌​‌‌​​​​‌​‌‌​​‌‌​​‌‌​​​‌​​‌‌​​​‌‌​​‌​‌‌​‌​​‌‌​​​‌​​‌‌​​​​​​‌‌​‌‌​​​‌‌​​‌‌​​‌​‌‌​‌​​‌‌​‌​​​‌‌​​​‌​​​‌‌​‌​‌​​‌‌​​‌‌​​‌​‌‌​‌​‌‌​​​‌​​‌‌​​‌‌​​​‌‌​​‌‌​​‌‌​​​​​​‌​‌‌​‌​‌‌​​‌​‌​‌‌​​​‌​​​‌‌​​‌‌​​‌‌​​‌‌​​‌‌​‌​​​​‌‌​​​‌​​‌‌‌​​​​​‌‌​‌​​​‌‌​​​​‌​​‌‌​‌​​​​‌‌​​​‌​‌‌​​‌​‌​​‌‌‌​‌​​​‌‌​​​‌​​‌‌​‌‌‌​​‌‌​‌‌‌​​‌‌​​‌‌​​‌‌‌​​‌​​‌‌​‌​​​​‌‌​​‌‌​​‌‌​‌‌‌​​‌‌​​‌​​​‌‌​​​​​​‌‌​​​​​​‌‌​‌‌‌​​‌‌​‌‌​

---
*Licensed to: m***c@g***l.com | AutoClaygent*