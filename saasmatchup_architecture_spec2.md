# SaaSMatchup — Directory Architecture Spec
*Hand this to Claude Code at the start of every session*

---

## What SaaSMatchup Is

A tech stack intelligence tool for marketing, data, and AI teams.
Two surfaces:
1. **Homepage quiz** → complete stack recommendation → result modal (already built)
2. **Directory** → browsable vendor listings organized by category and stack layer (building now)

The directory is NOT a tool review site. It is editorially opinionated architecture intelligence.
Every page should feel like a knowledgeable advisor wrote it, not a database exported it.

---

## Current State

- Live at: saasmatchup.com
- Built as: static HTML on Vercel
- Design system: cream/amber/Lora/Caveat/DM Sans (see tokens below)
- Stack diagram artifact exists: saasmatchup_stack_diagram.html (media company vertical)

---

## Design System — Never Deviate From These

```css
:root {
  --background: #FAF7F2;      /* cream/paper — warm, not clinical */
  --paper: #FFFDF9;           /* slightly lighter for cards */
  --accent: #D97706;          /* amber */
  --accent-light: #FDE68A;    /* amber light */
  --accent-pale: #FFFBEB;     /* amber pale */
  --text-primary: #1C1917;    /* near-black */
  --text-secondary: #44403C;  /* warm dark gray */
  --text-muted: #78716C;      /* warm medium gray */
  --text-faint: #A8A29E;      /* warm light gray */
  --border: rgba(28,25,23,0.12);
  --sketch-stroke: #292524;

  /* category accent colors */
  --rust: #C2410C;            /* order-to-cash / pain */
  --blue: #1D4ED8;            /* data engineering */
  --purple: #6D28D9;          /* agency / execution */
  --sage: #4D7C5F;            /* green / growth */

  --font-headline: 'Lora', serif;
  --font-ui: 'Caveat', cursive;
  --font-body: 'DM Sans', sans-serif;
}
```

**Signature interactions:**
- Buttons: amber offset box-shadow (4px 4px 0 #D97706)
- Card hover: dashed border + amber shadow
- Section eyebrows: Caveat font + amber underline rule
- Sketch/Excalidraw energy throughout — advisor not dashboard

**Google Fonts import (always include):**
```
https://fonts.googleapis.com/css2?family=Caveat:wght@400;500;600;700&family=Lora:ital,wght@0,400;0,500;0,600;1,400;1,500&family=DM+Sans:ital,wght@0,300;0,400;0,500;1,400&display=swap
```

---

## Site Structure

```
saasmatchup.com/
│
├── index.html                    ← homepage (quiz + hero) — DO NOT MODIFY
│
├── directory/
│   ├── index.html                ← directory hub (all 3 categories)
│   │
│   ├── martech/
│   │   ├── index.html            ← martech category hub
│   │   ├── attribution.html      ← DEEP — Sprint 1
│   │   ├── cdp-identity.html     ← DEEP — Sprint 4
│   │   ├── cem-lifecycle.html    ← DEEP — Sprint 7
│   │   ├── product-analytics.html ← DEEP — Sprint 9
│   │   ├── analytics-bi.html     ← DEEP — Sprint 12
│   │   └── adtech-dsp.html       ← MEDIUM
│   │
│   ├── data-engineering/
│   │   ├── index.html            ← data engineering category hub
│   │   ├── warehouse.html        ← DEEP — Sprint 2
│   │   ├── ingestion-elt.html    ← DEEP — Sprint 8
│   │   ├── transformation.html   ← DEEP — Sprint 11
│   │   ├── reverse-etl.html      ← DEEP — Sprint 5
│   │   └── orchestration.html    ← MEDIUM — Sprint 13
│   │
│   └── ai-ml/
│       ├── index.html            ← ai/ml category hub
│       ├── agents-orchestration.html ← DEEP — Sprint 3
│       ├── model-apis.html       ← DEEP — Sprint 6
│       ├── observability.html    ← DEEP — Sprint 10
│       ├── vector-databases.html ← MEDIUM
│       └── ai-for-marketing.html ← MEDIUM
│
├── tools/
│   └── [vendor-name].html        ← individual vendor pages (Sprint 4+)
│
├── compare/
│   └── [tool-a]-vs-[tool-b].html ← comparison pages (Sprint 4+)
│
└── get-help/
    └── index.html                ← consulting bridge → Clearpath Analytics
```

---

## Vendor Data Schema

All vendor data lives as a JavaScript array at the top of each category page.
Never hardcode vendor cards as HTML — always render from data.
This allows upgrading depth (light → medium → deep) by adding fields, not rewriting HTML.

```javascript
const vendors = [
  {
    // REQUIRED — all depth levels
    id: "vendor-slug",
    name: "Vendor Name",
    category: "martech",              // martech | data-engineering | ai-ml
    subcategory: "attribution",       // matches URL slug
    description: "One sentence. What it does, not what they claim.",
    bestFor: "Specific persona or use case",
    layer: "Measurement",             // where in the stack
    depth: "deep",                    // deep | medium | light
    url: "https://vendor.com",

    // MEDIUM + DEEP only
    pricing: "~$2K-5K/mo",           // real ranges, not "contact sales"
    worksWith: ["Snowflake", "GA4", "Shopify"],
    tags: ["attribution", "MTA", "incrementality"],

    // DEEP only
    whyItWins: "One sentence editorial opinion. Specific, not generic.",
    notFor: "Who should NOT use this",
    alternatives: ["Northbeam", "Rockerbox"],
    categoryTension: "attribution",   // links to the tension this vendor sits in
    highlight: false,                 // true = featured card treatment
  }
]
```

**Depth rendering rules:**
- `light` → name + description + bestFor + layer badge + external link
- `medium` → adds pricing + worksWith + tags
- `deep` → adds whyItWins + notFor + alternatives + featured styling option

---

## Page Types and Their Jobs

### 1. Category Hub Page (`/directory/martech/index.html`)
**Job:** Orient the visitor in < 10 seconds. Show all subcategories. Drive to subcategory pages or quiz.

**Sections:**
1. Header — category name + one-sentence editorial framing
2. Subcategory grid — card per subcategory with name, description, vendor count, CTA
3. "Not sure where to start?" → quiz CTA
4. Footer nav

### 2. Subcategory Page (`/directory/martech/attribution.html`)
**Job:** Be the definitive editorial resource for this category. Rank for "[category] tools 2026".

**Sections:**
1. Header — subcategory name + stack layer badge + vendor count
2. Category overview — 2-3 paragraphs. Opinionated. The tensions in the category. Where it's heading.
3. Filter bar — filter by depth/tag/use case (JS, no backend)
4. Vendor grid — cards rendered from data array, depth-aware
5. "The tensions" callout — named tensions in the category (e.g. "MTA vs MMM vs Incrementality")
6. Related categories — links to adjacent subcategory pages
7. CTA block — "Building this stack? Get a recommendation →" (quiz) + "Need implementation help? →" (get-help, only for martech/data-engineering pages)

### 3. Directory Hub Page (`/directory/index.html`)
**Job:** Entry point for all three categories. SEO landing page.

**Sections:**
1. Header — "The Stack Intelligence Directory"
2. Three category cards (MarTech / Data Engineering / AI-ML)
3. Featured subcategories — 2-3 highlighted from each
4. Quiz CTA

---

## Navigation Pattern

Every page needs a consistent nav:

```
[SaaSMatchup•]    Directory    Stacks    Get Help    [Take the Quiz →]
```

- Logo: Caveat font, amber dot
- Links: DM Sans, warm gray, amber underline on hover
- CTA button: amber background, offset box-shadow, Caveat font

---

## CTA Logic — Where to Send Traffic

```
On /directory/martech/* pages:
  Primary CTA   → quiz (saasmatchup.com/#quiz)
  Secondary CTA → get-help (only for attribution, cdp, cem, analytics-bi)
                  because these map to Clearpath services

On /directory/data-engineering/* pages:
  Primary CTA   → quiz
  Secondary CTA → get-help (warehouse, ingestion, transformation, reverse-etl)

On /directory/ai-ml/* pages:
  Primary CTA   → quiz
  Secondary CTA → none (Clearpath doesn't cover AI/ML infra)

On /get-help/ page:
  Primary action → clearpath-analytics.vercel.app
  Framing       → "Specialists we trust for implementation"
                  Editorial recommendation, not an ad
                  Disclose the relationship in one line
```

---

## SEO Requirements — Every Page

```html
<!-- Required on every page -->
<title>[Subcategory] Tools — SaaSMatchup Stack Intelligence</title>
<meta name="description" content="[2 sentence editorial description. Opinionated. Not generic.]">
<meta property="og:title" content="[Same as title]">
<meta property="og:description" content="[Same as meta description]">
<link rel="canonical" href="https://saasmatchup.com/directory/[category]/[subcategory]">

<!-- Schema markup on vendor pages -->
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "ItemList",
  "name": "[Subcategory] Tools",
  "numberOfItems": [count],
  "itemListElement": [/* vendor list */]
}
</script>
```

---

## Consulting Bridge — get-help Page

This page is editorially framed, not promotional.

```
Heading: "Need help implementing your stack?"
Subhead: "These are the specialists we recommend for marketing 
          data, attribution, and data engineering work."

[Card — Clearpath Analytics]
Angshuman Rudra
Ex-TapClicks founding PM · Yahoo · Adobe · Cornell MBA + MS CS
Specializes in: Attribution modeling, MMM, incrementality testing,
marketing data engineering, omni-channel dashboards

→ clearpath-analytics.vercel.app

Disclosure (small, honest):
"Full disclosure: Clearpath Analytics is run by SaaSMatchup's founder.
We only recommend them for work that maps directly to their expertise."
```

---

## Sprint Sequence

```
Sprint 1  → /directory/martech/attribution.html
Sprint 2  → /directory/data-engineering/warehouse.html
Sprint 3  → /directory/ai-ml/agents-orchestration.html   ← ADK first mover
Sprint 4  → /directory/martech/cdp-identity.html
Sprint 5  → /directory/data-engineering/reverse-etl.html  ← pair with CDP
Sprint 6  → /directory/ai-ml/model-apis.html
Sprint 7  → /directory/martech/cem-lifecycle.html
Sprint 8  → /directory/data-engineering/ingestion-elt.html
Sprint 9  → /directory/martech/product-analytics.html
Sprint 10 → /directory/ai-ml/observability.html
Sprint 11 → /directory/data-engineering/transformation.html
Sprint 12 → /directory/martech/analytics-bi.html
Sprint 13 → /directory/data-engineering/orchestration.html
Sprint 14 → remaining MEDIUM pages + /tools/ + /compare/
```

---

## What NOT To Do

- Never use localStorage or sessionStorage
- Never build a backend for the directory — it's all static HTML + JS
- Never hardcode vendor cards as HTML — always render from JS data array
- Never break the design system (no Inter, no blue gradients, no dark backgrounds)
- Never add a category that doesn't map to the marketing/data/AI buyer persona
- Never surface Clearpath Analytics on AI/ML pages — only on martech/data-engineering
- Never show placeholder stats (vendor counts must be real)
- Never add HRTech or ERP categories without explicit instruction
