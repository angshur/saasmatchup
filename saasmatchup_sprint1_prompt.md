# SaaSMatchup — Sprint 1 Build Prompt
# /directory/martech/attribution.html
*Give this to Claude Code along with saasmatchup_architecture_spec.md*

---

## Context

Read saasmatchup_architecture_spec.md first. Everything in that document applies.

This sprint builds ONE page: `/directory/martech/attribution.html`

This is the most important page on the site.
- Highest domain expertise (Clearpath Analytics is built on this knowledge)
- Best interview proof-of-work for: Measured, any adtech role, any measurement role
- Highest Clearpath consulting lead potential
- Establishes the template every subsequent sprint follows

---

## What To Build

A single self-contained HTML file: `attribution.html`

It must:
1. Match the SaaSMatchup design system exactly (cream, amber, Lora, Caveat, DM Sans)
2. Render vendor cards from a JavaScript data array (not hardcoded HTML)
3. Have a working filter bar (filter by tag, client size, no backend)
4. Be deployable to Vercel as a static file with no dependencies
5. Load Google Fonts from the CDN (no local fonts)
6. Have correct SEO meta tags
7. Have a nav bar consistent with the rest of the site

---

## Page Sections — Build In This Order

### Section 1 — Page Header
```
Eyebrow (Caveat font, amber):  "MarTech Directory → Attribution & Measurement"
Headline (Lora serif):         "Marketing Attribution & Measurement Tools"
Subhead (DM Sans):             "12 tools across MTA, MMM, incrementality, 
                                and unified measurement — with honest 
                                opinions on when each approach actually works."
Stack layer badge:             "Measurement Layer" — amber pill
```

### Section 2 — Category Overview (editorial, 3 paragraphs)

Use this copy verbatim — it is the editorial voice of the page:

**Paragraph 1 — The problem:**
"Attribution is a solved problem for last-click and an unsolved problem for everything else. Every platform claims credit. Google says ROAS 4.2. Meta says 3.8. Your CRM shows neither. The CFO asks which channel to cut, and you don't have a clean answer. That's not a data problem — you have plenty of data. It's a measurement problem."

**Paragraph 2 — The three approaches:**
"There are three fundamentally different ways to answer 'what's working': Multi-touch attribution (MTA) assigns fractional credit to touchpoints in the conversion path. Media Mix Modeling (MMM) uses statistical regression on historical spend and outcome data to estimate channel contribution. Incrementality testing runs controlled experiments — holdout groups — to measure the actual causal lift from a channel or campaign. Each answers a different question. Each has different data requirements, cost, and timeline. The mistake most teams make is picking one and assuming it covers everything."

**Paragraph 3 — The honest answer:**
"For most teams under $5M in annual ad spend: start with a solid MTA setup and one incrementality test per quarter. For teams between $5M-$50M: add lightweight MMM to your MTA — the two methods triangulate on truth better than either alone. Above $50M: you need all three, and you need someone who can reconcile when they disagree. The tools below are organized by which approach they support, with honest notes on who each one is actually built for."

### Section 3 — The Tensions Callout

A visually distinct box (dashed amber border, cream background, Caveat heading):

```
Heading: "The tensions in this category"

Tension 1: MTA vs MMM vs Incrementality
"These are not competing products — they answer different questions.
 Teams get into trouble when they treat them as substitutes."

Tension 2: Platform-reported vs independent measurement
"Google and Meta will always report better ROAS than independent 
 measurement. The gap between platform-reported and independently 
 measured is the number you actually need to know."

Tension 3: Speed vs accuracy
"Last-click is wrong but instant. MMM is more accurate but takes 
 weeks to run. Incrementality tests take months to design and execute. 
 Your measurement approach has to match your decision cadence."
```

### Section 4 — Filter Bar

Simple JS filter, no backend. Filter buttons:

```
All  |  MTA  |  MMM  |  Incrementality  |  Unified  |  Self-serve  |  Enterprise
```

Active filter: amber background, white text
Inactive: cream background, amber border

### Section 5 — Vendor Grid

Render from the vendor data array below.
Deep cards: 3-column grid on desktop, 1-column on mobile.
Card anatomy (deep depth):
- Vendor name (Lora serif, 18px)
- One-line description (DM Sans, 14px, muted)
- Tags (Caveat, small pills, amber border)
- "Best for" (DM Sans, 13px, with icon)
- Pricing (DM Sans, 13px, amber text)
- Works with (small tool name chips)
- "Why it wins" (DM Sans, 14px, ink-soft, slight left amber border accent)
- "Not for" (DM Sans, 13px, muted, rust color)
- External link: "Visit [Vendor] ↗" (amber, Caveat)

### Section 6 — Related Categories

```
→ CDP & Identity Resolution    (feeds attribution with identity data)
→ Analytics & BI               (where attribution output gets visualized)
→ Data Warehouse               (where measurement data lives)
→ Reverse ETL                  (how attribution segments reach activation)
```

### Section 7 — CTA Block

Two CTAs side by side:

```
Primary:   "Get a personalized stack recommendation →"
           Links to: saasmatchup.com/#quiz
           Style: amber button, offset shadow

Secondary: "Need help implementing attribution? →"  
           Links to: /get-help/
           Style: outline button, amber border
           Subtext: "Clearpath Analytics specializes in 
                     MTA, MMM, and incrementality"
```

---

## Vendor Data Array

Copy this exactly into the JavaScript at the top of the page:

```javascript
const vendors = [

  // ── MTA (Multi-Touch Attribution) ──────────────────────────────

  {
    id: "northbeam",
    name: "Northbeam",
    category: "martech",
    subcategory: "attribution",
    description: "Multi-touch attribution built for DTC and ecommerce brands running paid social at scale.",
    bestFor: "DTC brands spending $500K–$10M/yr on paid social and search",
    layer: "Measurement",
    depth: "deep",
    url: "https://northbeam.io",
    pricing: "~$2K–6K/mo",
    worksWith: ["Shopify", "Meta Ads", "Google Ads", "TikTok"],
    tags: ["MTA", "DTC", "paid social", "ecommerce"],
    whyItWins: "Faster to accurate MTA than most competitors — teams see usable data within days not weeks. Strongest on paid social triangulation for Shopify-based DTC brands.",
    notFor: "B2B, lead gen, or teams without a clear ecommerce data layer",
    alternatives: ["Rockerbox", "Triple Whale", "Measured"],
    highlight: true,
  },

  {
    id: "rockerbox",
    name: "Rockerbox",
    category: "martech",
    subcategory: "attribution",
    description: "Multi-touch attribution and marketing data unification for mid-market brands.",
    bestFor: "Mid-market brands that need MTA + data centralization in one platform",
    layer: "Measurement",
    depth: "deep",
    url: "https://rockerbox.com",
    pricing: "~$2K–5K/mo",
    worksWith: ["Snowflake", "GA4", "Shopify", "Salesforce"],
    tags: ["MTA", "data unification", "mid-market"],
    whyItWins: "Stronger data centralization story than Northbeam — if you want MTA AND a clean marketing data foundation, Rockerbox does both. Better for teams with a data warehouse.",
    notFor: "Teams looking for pure incrementality or MMM — this is MTA-first",
    alternatives: ["Northbeam", "Triple Whale", "Measured"],
    highlight: false,
  },

  {
    id: "triple-whale",
    name: "Triple Whale",
    category: "martech",
    subcategory: "attribution",
    description: "Attribution and analytics dashboard purpose-built for Shopify DTC brands.",
    bestFor: "Shopify DTC brands under $10M revenue wanting fast attribution setup",
    layer: "Measurement",
    depth: "deep",
    url: "https://triplewhale.com",
    pricing: "~$200–2K/mo (usage-based)",
    worksWith: ["Shopify", "Meta Ads", "Google Ads", "Klaviyo"],
    tags: ["MTA", "DTC", "Shopify", "self-serve"],
    whyItWins: "Lowest barrier to entry for DTC attribution. If you're on Shopify and need something working this week, Triple Whale gets you there. Weakest at enterprise data exports.",
    notFor: "Teams not on Shopify, or anyone needing warehouse-native data",
    alternatives: ["Northbeam", "Rockerbox"],
    highlight: false,
  },

  // ── INCREMENTALITY ─────────────────────────────────────────────

  {
    id: "measured",
    name: "Measured",
    category: "martech",
    subcategory: "attribution",
    description: "Incrementality testing platform — holdout-based measurement of true causal lift by channel.",
    bestFor: "Brands spending $5M+/yr on paid media that have outgrown MTA",
    layer: "Measurement",
    depth: "deep",
    url: "https://measured.com",
    pricing: "~$5K–15K/mo (enterprise)",
    worksWith: ["Meta Ads", "Google Ads", "The Trade Desk", "Snowflake"],
    tags: ["incrementality", "holdout testing", "causal measurement", "enterprise"],
    whyItWins: "The clearest answer to 'what is actually incremental' — holdout methodology is more defensible to a CFO than any MTA model. Best for teams that need to justify large budget reallocations.",
    notFor: "Teams under $2M ad spend — the statistical power requirements make it overkill",
    alternatives: ["Northbeam", "Meta Conversion Lift", "GeoLift (open source)"],
    highlight: true,
  },

  {
    id: "geolift",
    name: "GeoLift (Meta Open Source)",
    category: "martech",
    subcategory: "attribution",
    description: "Open source geo-based incrementality testing framework from Meta.",
    bestFor: "Data teams that want to run incrementality tests without vendor cost",
    layer: "Measurement",
    depth: "medium",
    url: "https://github.com/facebookincubator/GeoLift",
    pricing: "Free (open source) — requires data engineering time",
    worksWith: ["R", "Python", "Snowflake", "BigQuery"],
    tags: ["incrementality", "open source", "geo testing", "self-serve"],
    whyItWins: "Zero licensing cost. If you have a data engineer and 2 weeks, this gets you holdout-based incrementality without the enterprise price tag.",
    notFor: "Teams without data engineering capacity",
    alternatives: ["Measured", "Northbeam"],
    highlight: false,
  },

  // ── MMM (Media Mix Modeling) ────────────────────────────────────

  {
    id: "meridian",
    name: "Meridian (Google)",
    category: "martech",
    subcategory: "attribution",
    description: "Open source Bayesian MMM framework from Google, released 2024.",
    bestFor: "Data science teams that want a defensible, open-source MMM baseline",
    layer: "Measurement",
    depth: "deep",
    url: "https://developers.google.com/meridian",
    pricing: "Free (open source) — requires data science capacity",
    worksWith: ["BigQuery", "Python", "Google Ads"],
    tags: ["MMM", "open source", "Bayesian", "Google"],
    whyItWins: "Google's own MMM framework — defensible methodology, transparent assumptions, no vendor lock-in. The right starting point for any team building MMM in-house.",
    notFor: "Teams without a data scientist. This is not a SaaS product — it is a framework.",
    alternatives: ["Robyn (Meta)", "Lightweight MMM (Google)", "Recast"],
    highlight: false,
  },

  {
    id: "robyn",
    name: "Robyn (Meta Open Source)",
    category: "martech",
    subcategory: "attribution",
    description: "Open source MMM framework from Meta, widely adopted since 2021.",
    bestFor: "Data teams building MMM with heavy Meta Ads spend",
    layer: "Measurement",
    depth: "medium",
    url: "https://facebookexperimental.github.io/Robyn/",
    pricing: "Free (open source)",
    worksWith: ["R", "Python", "Meta Ads API"],
    tags: ["MMM", "open source", "Meta", "R"],
    whyItWins: "Largest open source MMM community. Strong documentation and practitioner community. Natural fit if Meta is a primary channel.",
    notFor: "Teams that want a managed SaaS MMM — this requires R/Python capacity",
    alternatives: ["Meridian", "Recast"],
    highlight: false,
  },

  {
    id: "recast",
    name: "Recast",
    category: "martech",
    subcategory: "attribution",
    description: "Managed Bayesian MMM as a service — no data science team required.",
    bestFor: "Mid-market brands that want MMM insights without building in-house",
    layer: "Measurement",
    depth: "deep",
    url: "https://getrecast.com",
    pricing: "~$3K–8K/mo",
    worksWith: ["Google Ads", "Meta Ads", "Snowflake", "CSV export"],
    tags: ["MMM", "managed service", "Bayesian", "mid-market"],
    whyItWins: "Best managed MMM for teams that want the methodology without the data science overhead. Faster to first insights than building Robyn or Meridian in-house.",
    notFor: "Teams that need custom model assumptions or want full methodology transparency",
    alternatives: ["Meridian", "Robyn", "Northbeam"],
    highlight: false,
  },

  // ── UNIFIED MEASUREMENT ─────────────────────────────────────────

  {
    id: "klaviyo-attribution",
    name: "Nielsen Attribution (formerly Visual IQ)",
    category: "martech",
    subcategory: "attribution",
    description: "Enterprise MTA and unified measurement for large advertisers.",
    bestFor: "Enterprise brands with $50M+ ad spend needing cross-channel unified measurement",
    layer: "Measurement",
    depth: "medium",
    url: "https://nielsen.com",
    pricing: "Enterprise — contact sales",
    worksWith: ["DV360", "The Trade Desk", "Salesforce", "Adobe"],
    tags: ["MTA", "unified measurement", "enterprise"],
    whyItWins: "Strongest for enterprise cross-media measurement including TV and offline. Nielsen's panel data integration is unique.",
    notFor: "Mid-market or DTC — overkill in cost and complexity",
    alternatives: ["Measured", "Rockerbox"],
    highlight: false,
  },

  {
    id: "northstar",
    name: "Analytic Edge / NorthStar",
    category: "martech",
    subcategory: "attribution",
    description: "Unified measurement combining MTA, MMM, and incrementality in one framework.",
    bestFor: "Sophisticated marketing teams wanting to triangulate across all three methodologies",
    layer: "Measurement",
    depth: "medium",
    url: "https://analyticedge.com",
    pricing: "Enterprise",
    worksWith: ["Snowflake", "BigQuery", "major ad platforms"],
    tags: ["unified measurement", "MMM", "MTA", "incrementality"],
    whyItWins: "One of few platforms that genuinely triangulates MTA + MMM + incrementality. Useful when methods disagree and you need to reconcile.",
    notFor: "Teams early in measurement maturity — start with one methodology first",
    alternatives: ["Measured", "Recast", "Meridian"],
    highlight: false,
  },

  // ── SELF-SERVE / EMERGING ───────────────────────────────────────

  {
    id: "attributer",
    name: "Attributer",
    category: "martech",
    subcategory: "attribution",
    description: "Lightweight UTM-based attribution for B2B lead gen teams.",
    bestFor: "B2B SaaS teams that need basic channel attribution without a data engineer",
    layer: "Measurement",
    depth: "medium",
    url: "https://attributer.io",
    pricing: "~$49–199/mo",
    worksWith: ["HubSpot", "Salesforce", "Google Analytics", "Webflow"],
    tags: ["UTM", "B2B", "self-serve", "lead gen"],
    whyItWins: "Simplest possible attribution for B2B — captures UTM data into CRM fields automatically. Not sophisticated, but gets the basics right for teams not ready for MTA.",
    notFor: "Ecommerce, DTC, or anyone needing cross-device or cross-channel modeling",
    alternatives: ["Rockerbox", "HubSpot native attribution"],
    highlight: false,
  },

  {
    id: "posthog-attribution",
    name: "PostHog",
    category: "martech",
    subcategory: "attribution",
    description: "Product analytics platform with built-in attribution and session replay.",
    bestFor: "PLG SaaS teams that want attribution tied to product usage data",
    layer: "Measurement",
    depth: "medium",
    url: "https://posthog.com",
    pricing: "Free tier + usage-based (~$0–2K/mo for most teams)",
    worksWith: ["Snowflake", "BigQuery", "Stripe", "Hubspot"],
    tags: ["product analytics", "PLG", "self-serve", "open source"],
    whyItWins: "Only attribution-adjacent tool that natively connects marketing source to product behavior. Invaluable for PLG companies measuring activation by acquisition channel.",
    notFor: "Ecommerce or brand advertising measurement — this is product-first",
    alternatives: ["Amplitude", "Mixpanel", "Heap"],
    highlight: false,
  },

];
```

---

## Filter Logic

```javascript
// Filter tags that appear in the filter bar
const filterTags = ["All", "MTA", "MMM", "Incrementality", "Unified", "Self-serve", "Enterprise"];

// Filter function
function filterVendors(tag) {
  if (tag === "All") return vendors;
  if (tag === "Self-serve") return vendors.filter(v => v.tags.includes("self-serve"));
  if (tag === "Enterprise") return vendors.filter(v => v.tags.includes("enterprise"));
  return vendors.filter(v => v.tags.some(t => t.toLowerCase() === tag.toLowerCase()));
}

// Re-render grid on filter click
// Animate cards out/in on filter change (simple opacity + translateY)
```

---

## SEO Meta Tags — Use Exactly

```html
<title>Marketing Attribution Tools 2026 — MTA, MMM & Incrementality | SaaSMatchup</title>
<meta name="description" content="Honest comparison of 12 marketing attribution tools across MTA, MMM, and incrementality testing. Opinionated guidance on which methodology fits your spend level and team — not just a feature list.">
<meta property="og:title" content="Marketing Attribution Tools 2026 — SaaSMatchup">
<meta property="og:description" content="MTA vs MMM vs incrementality — which attribution approach fits your team. 12 tools with honest positioning.">
<link rel="canonical" href="https://saasmatchup.com/directory/martech/attribution">
```

---

## Nav Bar — Exact Markup Pattern

```html
<nav>
  <a href="/" class="logo">
    <span class="logo-text">SaaSMatchup</span>
    <span class="logo-dot"></span>
  </a>
  <div class="nav-links">
    <a href="/directory/">Directory</a>
    <a href="/stacks/">Stacks</a>
    <a href="/get-help/">Get Help</a>
  </div>
  <a href="/#quiz" class="nav-cta">Take the Quiz →</a>
</nav>
```

---

## Breadcrumb — Below Nav

```
Home → Directory → MarTech → Attribution & Measurement
```

Each segment links to its page. Caveat font, muted color, amber chevrons.

---

## Output

Produce a single file: `attribution.html`

It must open correctly in a browser as a local file (no server required).
All fonts from Google CDN.
No external JS dependencies.
Vanilla JS only for filtering.
Fully mobile responsive.

After building, Claude Code should:
1. Verify all 12 vendor cards render correctly
2. Verify filter bar works for all 7 filter options
3. Verify both CTAs link correctly
4. Verify mobile layout on 375px viewport

---

## What This Page Unlocks

This page is the template for every subsequent sprint.
Once it's built and the data-driven card pattern is working,
each future sprint is: update vendor data array + update copy + update meta tags.

The page structure, design system, filter logic, and card components
are reused across all 14 sprints.
