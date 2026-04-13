
=============================================================================
  SaaSMatchup.com — Full Build Specification
  Version 1.0 | For Cursor / Lovable / Bolt
  Focus: Marketing Agencies + Media/Publishers | Goal: Validate + SEO + Revenue
=============================================================================

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 1 — STRATEGIC ARCHITECTURE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Core Positioning
  "Stack architecture intelligence" — not a tool directory, not a review site.
  SaaSMatchup tells you WHICH tools to use TOGETHER for YOUR company type.

Three-Sided Marketplace
  Side 1: Buying companies (get free stack recommendations)
  Side 2: SaaS vendors (get featured placement + leads)
  Side 3: Consultants (get qualified buyer leads)

Cold Start Strategy
  → Pre-populate consultant directory with public profiles (manual curation)
     before charging. Directory must feel alive on day 1.
  → Build affiliate links into every stack page from launch.
     (HubSpot, Monday, Klaviyo, Fivetran, Datadog, Snowflake all have programs)
  → SEO pages ARE the validation surface. Real user engagement = validation signal.

MVP Scope (6-week build)
  ✓ 2 vertical stack guides (Marketing Agencies + Media/Publishers)
  ✓ 10 tool comparison pages
  ✓ 8 category pages
  ✓ Consultant directory (pre-populated, 20–50 profiles)
  ✓ AI Stack Builder quiz (5 questions → full stack output)
  ✓ Lead capture on every page
  ✓ Affiliate links wired up

NOTE: AI Stack Builder should be in MVP, not Phase 3.
  This is the primary moat. G2/Capterra cannot replicate multi-tool
  architecture recommendations. Even a simple quiz → stack output is
  differentiated from day one.


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 2 — TECH STACK
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  Layer               Tool                 Why
  ─────────────────── ──────────────────── ─────────────────────────────────────
  Frontend            Next.js 14 (App Dir) SEO-critical SSR/SSG; fast routing
  Styling             Tailwind CSS         Speed; design system out of the box
  UI Components       shadcn/ui            Production-grade, customizable
  Backend/Auth/DB     Supabase             PostgreSQL + Auth + Storage + Realtime
  Search              Algolia              Instant search across tools/stacks
  CMS (content)       Sanity.io            Headless; easy to update stack pages
  AI Stack Builder    OpenAI API (GPT-4o)  Structured JSON output for stack recs
  Affiliate tracking  custom UTM params    Wire into each tool link
  Email capture       Resend + React Email Welcome flows, lead nurture
  Analytics           PostHog              Product analytics + session replay
  Hosting             Vercel               SSG + edge functions; free tier OK


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 3 — FULL DATABASE SCHEMA (Supabase / PostgreSQL)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

-- SaaS TOOLS TABLE
CREATE TABLE tools (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  slug            TEXT UNIQUE NOT NULL,          -- 'hubspot', 'salesforce'
  name            TEXT NOT NULL,
  website         TEXT,
  tagline         TEXT,
  description     TEXT,
  logo_url        TEXT,
  screenshot_url  TEXT,
  l1_category     TEXT,                          -- 'Sales', 'Marketing', etc.
  l2_category     TEXT,                          -- 'CRM', 'Marketing Automation'
  pricing_model   TEXT,                          -- 'freemium', 'per-seat', 'usage'
  price_range     TEXT,                          -- '$', '$$', '$$$', '$$$$'
  starting_price  NUMERIC,                       -- monthly USD
  free_tier       BOOLEAN DEFAULT FALSE,
  g2_rating       NUMERIC(3,1),
  g2_reviews      INT,
  ai_native       TEXT,                          -- 'AI Native', 'Some AI', 'No'
  customer_size   TEXT,                          -- 'SMB', 'Mid-Market', 'Enterprise'
  public_company  BOOLEAN DEFAULT FALSE,
  affiliate_url   TEXT,                          -- your affiliate link
  affiliate_commission TEXT,
  sponsored       BOOLEAN DEFAULT FALSE,
  featured_rank   INT,
  created_at      TIMESTAMPTZ DEFAULT NOW(),
  updated_at      TIMESTAMPTZ DEFAULT NOW()
);

-- CATEGORIES TABLE
CREATE TABLE categories (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  slug            TEXT UNIQUE NOT NULL,
  name            TEXT NOT NULL,
  l1_parent       TEXT,                          -- 'Sales', 'Marketing', etc.
  description     TEXT,
  buyer_persona   TEXT,
  typical_price   TEXT,
  seo_title       TEXT,
  seo_description TEXT,
  tool_count      INT DEFAULT 0,
  created_at      TIMESTAMPTZ DEFAULT NOW()
);

-- VERTICALS (target industries/company types)
CREATE TABLE verticals (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  slug            TEXT UNIQUE NOT NULL,          -- 'marketing-agency', 'media-publisher'
  name            TEXT NOT NULL,
  description     TEXT,
  icon            TEXT,
  buyer_persona   TEXT,
  company_size    TEXT,
  primary_pain_points TEXT[],
  seo_title       TEXT,
  seo_description TEXT,
  hero_heading    TEXT,
  created_at      TIMESTAMPTZ DEFAULT NOW()
);

-- STACK TEMPLATES (the core product)
CREATE TABLE stacks (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  slug            TEXT UNIQUE NOT NULL,
  vertical_id     UUID REFERENCES verticals(id),
  name            TEXT NOT NULL,                 -- 'Marketing Agency Stack'
  subtitle        TEXT,
  description     TEXT,
  company_size    TEXT,                          -- 'SMB', 'Mid-Market', 'All'
  annual_budget   TEXT,                          -- '$50k-$200k', '$200k+'
  team_size       TEXT,
  difficulty      TEXT,                          -- 'Simple', 'Moderate', 'Complex'
  total_cost_est  TEXT,                          -- '$800-$2,500/mo'
  seo_title       TEXT,
  seo_description TEXT,
  intro_content   TEXT,                          -- long-form markdown
  integration_notes TEXT,
  implementation_guide TEXT,
  published       BOOLEAN DEFAULT FALSE,
  featured        BOOLEAN DEFAULT FALSE,
  view_count      INT DEFAULT 0,
  created_at      TIMESTAMPTZ DEFAULT NOW(),
  updated_at      TIMESTAMPTZ DEFAULT NOW()
);

-- STACK LAYERS (e.g. "Customer Layer", "Data Layer")
CREATE TABLE stack_layers (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  stack_id        UUID REFERENCES stacks(id) ON DELETE CASCADE,
  layer_name      TEXT NOT NULL,                 -- 'Customer Layer', 'Data Layer'
  layer_order     INT NOT NULL,
  description     TEXT
);

-- STACK COMPONENTS (tool recommendations within each layer)
CREATE TABLE stack_components (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  stack_id        UUID REFERENCES stacks(id) ON DELETE CASCADE,
  layer_id        UUID REFERENCES stack_layers(id),
  category_id     UUID REFERENCES categories(id),
  recommended_tool_id UUID REFERENCES tools(id),
  component_order INT DEFAULT 0,
  rationale       TEXT,                          -- why this tool for this stack
  alternative_tool_ids UUID[],                   -- array of tool IDs
  integration_notes TEXT,
  is_required     BOOLEAN DEFAULT TRUE,
  affiliate_priority BOOLEAN DEFAULT FALSE
);

-- TOOL COMPARISONS
CREATE TABLE comparisons (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  slug            TEXT UNIQUE NOT NULL,          -- 'hubspot-vs-salesforce'
  title           TEXT NOT NULL,
  tool_ids        UUID[],                        -- 2-4 tools
  verdict_winner  UUID REFERENCES tools(id),
  verdict_summary TEXT,
  detailed_comparison TEXT,                      -- markdown
  use_case_breakdown  TEXT,                      -- when to pick each
  seo_title       TEXT,
  seo_description TEXT,
  view_count      INT DEFAULT 0,
  published       BOOLEAN DEFAULT FALSE,
  created_at      TIMESTAMPTZ DEFAULT NOW(),
  updated_at      TIMESTAMPTZ DEFAULT NOW()
);

-- CONSULTANTS
CREATE TABLE consultants (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  slug            TEXT UNIQUE NOT NULL,
  name            TEXT NOT NULL,
  company         TEXT,
  title           TEXT,
  bio             TEXT,
  avatar_url      TEXT,
  location        TEXT,
  timezone        TEXT,
  website         TEXT,
  linkedin_url    TEXT,
  calendly_url    TEXT,
  email           TEXT,
  tool_ids        UUID[],                        -- tools they specialize in
  vertical_ids    UUID[],                        -- industries they serve
  specializations TEXT[],
  certifications  TEXT[],
  years_exp       INT,
  hourly_rate_min INT,
  hourly_rate_max INT,
  project_min     INT,                           -- min project size $
  customer_size   TEXT,
  rating          NUMERIC(3,1),
  review_count    INT DEFAULT 0,
  lead_price_usd  INT,                           -- what they pay per lead
  profile_tier    TEXT DEFAULT 'free',           -- 'free', 'basic', 'premium'
  verified        BOOLEAN DEFAULT FALSE,
  featured        BOOLEAN DEFAULT FALSE,
  created_at      TIMESTAMPTZ DEFAULT NOW()
);

-- LEADS (core revenue table)
CREATE TABLE leads (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  source_page     TEXT,                          -- which page generated lead
  source_type     TEXT,                          -- 'stack', 'comparison', 'category', 'quiz'
  vertical        TEXT,
  tools_interested TEXT[],
  company_name    TEXT,
  company_size    TEXT,
  contact_name    TEXT,
  contact_email   TEXT NOT NULL,
  contact_phone   TEXT,
  message         TEXT,
  budget_range    TEXT,
  timeline        TEXT,
  assigned_consultant_id UUID REFERENCES consultants(id),
  lead_status     TEXT DEFAULT 'new',            -- 'new', 'sent', 'accepted', 'closed'
  lead_value      INT,
  created_at      TIMESTAMPTZ DEFAULT NOW()
);

-- AI STACK BUILDER SESSIONS
CREATE TABLE quiz_sessions (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  session_token   TEXT UNIQUE NOT NULL,
  answers         JSONB,                         -- {industry, size, budget, channels, pain}
  generated_stack JSONB,                         -- full AI output
  email           TEXT,
  converted_lead  BOOLEAN DEFAULT FALSE,
  created_at      TIMESTAMPTZ DEFAULT NOW()
);

-- PAGE VIEWS / ANALYTICS (lightweight supplement to PostHog)
CREATE TABLE page_events (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  page_type       TEXT,                          -- 'stack', 'comparison', 'category'
  page_slug       TEXT,
  event_type      TEXT,                          -- 'view', 'affiliate_click', 'lead_form'
  tool_id         UUID REFERENCES tools(id),
  session_id      TEXT,
  created_at      TIMESTAMPTZ DEFAULT NOW()
);

-- VENDOR SPONSORSHIP
CREATE TABLE sponsorships (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  tool_id         UUID REFERENCES tools(id),
  sponsorship_type TEXT,                         -- 'category', 'stack', 'homepage', 'comparison'
  target_slug     TEXT,                          -- which page/category sponsored
  monthly_fee     INT,
  start_date      DATE,
  end_date        DATE,
  impressions     INT DEFAULT 0,
  clicks          INT DEFAULT 0,
  active          BOOLEAN DEFAULT TRUE,
  created_at      TIMESTAMPTZ DEFAULT NOW()
);


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 4 — SITE ARCHITECTURE & URL STRUCTURE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  URL                                        Page Type         Priority
  ──────────────────────────────────────────────────────────────────────────
  /                                          Homepage          P0
  /stacks                                    Stack index       P0
  /stacks/marketing-agency                   Stack page        P0
  /stacks/media-publisher                    Stack page        P0
  /stacks/saas-company                       Stack page        P1
  /stacks/ecommerce                          Stack page        P1
  /compare                                   Compare index     P0
  /compare/hubspot-vs-salesforce             Comparison        P0
  /compare/klaviyo-vs-mailchimp              Comparison        P0
  /compare/fivetran-vs-airbyte               Comparison        P0
  /compare/tapclicks-vs-ninjaCat             Comparison        P0 (your niche!)
  /compare/snowflake-vs-databricks           Comparison        P1
  /compare/datadog-vs-new-relic              Comparison        P1
  /categories                                Category index    P1
  /categories/crm                            Category page     P0
  /categories/marketing-automation           Category page     P0
  /categories/data-analytics                 Category page     P0
  /categories/etl-data-integration           Category page     P1
  /categories/cdp                            Category page     P1
  /tools/[slug]                              Tool profile      P1
  /consultants                               Directory         P0
  /consultants/[slug]                        Profile           P1
  /consultants/hubspot                       Tool-filtered     P0
  /consultants/marketing-automation          Specialty filter  P0
  /quiz                                      AI Stack Builder  P0
  /blog                                      Blog index        P1
  /blog/[slug]                               Blog post         P1


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 5 — PAGE TEMPLATES (Component Specs for Each Page Type)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

TEMPLATE A: STACK PAGE (/stacks/marketing-agency)
──────────────────────────────────────────────────
  Section 1: Hero
    - H1: "Best SaaS Stack for Marketing Agencies in 2025"
    - Subheading: what problem this solves
    - Quick stats: "7 tools | $1,200-$3,500/mo | Works for 10-200 person agencies"
    - CTA: "Get a custom stack for your agency →" (leads to quiz)

  Section 2: Stack Overview Card
    - Visual layer diagram (Customer → Data → Analytics → Execution)
    - Each layer: tool logo + name + one-liner

  Section 3: Recommended Tools (by layer)
    For each tool:
    - Logo, name, category, star rating, price range
    - 2-sentence rationale: "Why this tool for agencies"
    - Affiliate CTA button: "Try [Tool] →" (tracked affiliate link)
    - "See alternatives" toggle

  Section 4: Alternatives Table
    - Side-by-side: Recommended vs Alt 1 vs Alt 2
    - Columns: Price, Best for, G2 Rating, Free tier

  Section 5: Integration Map
    - Text: which tools connect natively, which need Zapier/ETL
    - Integration complexity rating

  Section 6: Total Cost Estimate
    - Table: Tool | Plan | $/mo | $/yr
    - Total row
    - "Scale" variant (if agency grows)

  Section 7: Consultant CTA
    - "Need help implementing this stack?"
    - 3 consultant cards (filtered to agency + tools in stack)
    - Lead form

  Section 8: FAQ (schema markup for SEO)
    - 5-8 questions targeting long-tail keywords

  Section 9: Related Stacks + Related Comparisons


TEMPLATE B: COMPARISON PAGE (/compare/hubspot-vs-salesforce)
─────────────────────────────────────────────────────────────
  Section 1: Hero
    - H1: "HubSpot vs Salesforce: Which CRM is Right for Your Business?"
    - Verdict badge: "Best for SMBs: HubSpot | Best for Enterprise: Salesforce"
    - Quick comparison table (top 6 criteria)

  Section 2: Side-by-side feature table
    - Pricing
    - Ease of use
    - Integrations
    - AI features
    - Customer support
    - Mobile app
    - Customization
    - Reporting

  Section 3: "When to Choose Each" cards
    - HubSpot if: [list of scenarios]
    - Salesforce if: [list of scenarios]

  Section 4: Detailed breakdown (expandable sections)
    - Pricing deep-dive
    - Features comparison
    - Implementation complexity
    - Support quality

  Section 5: User reviews summary (from public data)

  Section 6: Affiliate CTAs for both tools

  Section 7: Related comparisons
    - "Also compare: HubSpot vs Pipedrive", "Salesforce vs Microsoft Dynamics"

  Section 8: Consultant CTA (filtered to these tools)


TEMPLATE C: CATEGORY PAGE (/categories/crm)
─────────────────────────────────────────────
  Section 1: Hero
    - H1: "Best CRM Software in 2025"
    - What is CRM (schema: DefinedTerm)
    - Who buys it, typical price range

  Section 2: Quick Picks
    - Best overall: HubSpot
    - Best for enterprise: Salesforce
    - Best value: Pipedrive
    - Best AI-native: [tool]

  Section 3: Full tool cards (8-12 tools)
    - Logo, name, tagline, pricing, rating, affiliate CTA

  Section 4: Comparison table (top 6 tools)

  Section 5: Buyer's guide
    - What to look for in a CRM
    - Questions to ask vendors
    - Implementation checklist

  Section 6: Which stack does this fit into?
    - Links to relevant stack pages

  Section 7: FAQ + consultant CTA


TEMPLATE D: AI STACK BUILDER (/quiz)
──────────────────────────────────────
  Step 1: "What type of company are you?"
    - Marketing Agency
    - Media / Publisher
    - SaaS Company
    - eCommerce
    - Other

  Step 2: "How big is your team?"
    - 1-10 | 11-50 | 51-200 | 200+

  Step 3: "What's your annual SaaS budget?"
    - Under $50k | $50k-$200k | $200k-$500k | $500k+

  Step 4: "What are your biggest operational pain points?" (multi-select)
    - Managing client reporting
    - Too many disconnected tools
    - Poor data visibility
    - Scaling campaigns
    - Attribution / ROI tracking
    - Hiring & onboarding
    - Finance / billing ops

  Step 5: "Which tools are you currently using?" (optional)

  Output:
    - Full stack recommendation (5-8 tools)
    - Each tool: name + logo + why + affiliate link
    - Total estimated cost
    - "Email me this stack" → lead capture
    - "Find a consultant to help me build this" → consultant CTA


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 6 — AI STACK BUILDER PROMPT (OpenAI API)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

System prompt:
  You are a B2B SaaS stack advisor. Your job is to recommend the optimal
  technology stack for a company based on their profile.
  Always return a valid JSON object matching the schema below.
  Never include tools that don't integrate well together.
  Prioritize commonly adopted, production-proven tools.
  Include one "hidden gem" pick per stack that is underrated.

User prompt template:
  Company profile:
  - Type: {company_type}
  - Team size: {team_size}
  - Annual SaaS budget: {budget}
  - Pain points: {pain_points}
  - Current tools: {current_tools}

  Return a JSON object with this structure:
  {
    "stack_name": "Marketing Agency Stack",
    "summary": "2-3 sentence overview",
    "total_cost_estimate": "$1,200-$2,800/mo",
    "layers": [
      {
        "layer_name": "Customer & CRM Layer",
        "tools": [
          {
            "name": "HubSpot",
            "category": "CRM + Marketing Automation",
            "why": "Why this tool specifically for their profile",
            "plan_recommendation": "HubSpot Marketing Hub Professional",
            "estimated_cost": "$800/mo",
            "affiliate_slug": "hubspot",
            "hidden_gem": false
          }
        ]
      }
    ],
    "integration_notes": "How these tools connect",
    "implementation_order": ["HubSpot", "Fivetran", "Snowflake", "Looker"],
    "risks": ["Risk 1", "Risk 2"],
    "next_steps": ["Step 1", "Step 2", "Step 3"]
  }


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 7 — SEO CONTENT PLAN (First 90 Days)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Priority 1 — Stack Pages (highest intent, lowest competition)
  URL                           Target Keyword
  ───────────────────────────── ────────────────────────────────────────
  /stacks/marketing-agency      "best saas stack for marketing agencies"
  /stacks/media-publisher       "best tools for media companies"
  /stacks/marketing-agency      "martech stack for agencies"
  /stacks/media-publisher       "media company tech stack"

Priority 2 — Comparison Pages (high intent, moderate competition)
  URL                              Target Keyword                 Monthly Vol (est)
  ──────────────────────────────── ────────────────────────────── ─────────────────
  /compare/hubspot-vs-salesforce   hubspot vs salesforce          40,000+
  /compare/klaviyo-vs-mailchimp    klaviyo vs mailchimp           12,000+
  /compare/fivetran-vs-airbyte     fivetran vs airbyte            3,000
  /compare/tapclicks-vs-ninjaCat   tapclicks vs ninjaCat          400 (your niche!)
  /compare/datadog-vs-new-relic    datadog vs new relic           8,000
  /compare/snowflake-vs-databricks snowflake vs databricks        5,000

Priority 3 — Category Pages (broad, build over time)
  URL                              Target Keyword
  ──────────────────────────────── ──────────────────────────────
  /categories/crm                  best crm software 2025
  /categories/marketing-automation best marketing automation tools
  /categories/data-analytics       marketing analytics platforms
  /categories/etl-data-integration best etl tools saas

Priority 4 — Long-tail blog posts (month 2-3)
  Title                                                 Intent
  ───────────────────────────────────────────────────── ──────────────────
  "HubSpot for Agencies: The Complete 2025 Guide"       High / Affiliate
  "How to Build a Data Stack for a Media Company"       High / Niche
  "TapClicks vs NinjaCat: Agency Reporting Deep Dive"   Very High / Yours
  "Fivetran vs Airbyte: Which ETL Tool is Right?"       High / Affiliate
  "The Complete Marketing Agency Tech Stack in 2025"    Very High
  "Best Analytics Tools for Digital Publishers 2025"    High

On-page SEO requirements for every page:
  ✓ H1 targeting primary keyword
  ✓ FAQ schema (JSON-LD)
  ✓ Product schema for each tool reviewed
  ✓ Breadcrumb schema
  ✓ Internal linking: stack pages → comparison pages → category pages
  ✓ Meta title under 60 chars
  ✓ Meta description 140-160 chars with CTA
  ✓ Open Graph image (auto-generated per page)
  ✓ Sitemap.xml auto-generated by Next.js
  ✓ robots.txt
  ✓ Page speed target: LCP < 2.5s (use SSG/ISR)


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 8 — MONETIZATION SEQUENCING
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Month 1-2: Affiliate (fastest path to revenue)
  → Wire affiliate links into every tool recommendation
  → Prioritize tools with programs: HubSpot, Monday, Klaviyo, Shopify,
    Datadog, Fivetran, Snowflake, ClickUp, Pipedrive
  → Expected: $0 for 60 days while SEO builds; then $200-$2k/mo

Month 2-3: Consultant lead marketplace (manual first)
  → Build directory, manually match buyers to consultants
  → Email consultants directly: "I have a lead for you, $150"
  → Don't build payment system yet — use Stripe links manually
  → Expected: 2-5 leads/mo initially

Month 3-4: Vendor sponsorship (outbound only)
  → Target: tools in the niches you cover (TapClicks, Fivetran, etc.)
  → Pitch: "We rank for [keyword your ICP searches]. Sponsor the category."
  → Price: $500-$2,000/mo per category sponsorship
  → Expected: 1-3 sponsors after 90 days of traffic

Revenue targets (conservative):
  Month 3:  $500   (affiliate only)
  Month 6:  $3,000 (affiliate + 3-5 leads + 1 sponsor)
  Month 12: $15,000 (all three channels firing)


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 9 — CURSOR / BOLT PROMPT (paste this to start building)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

--- PASTE THIS INTO CURSOR OR BOLT ---

Build a Next.js 14 web application called SaaSMatchup.com.

CONCEPT:
A three-sided marketplace that recommends SaaS technology stacks for
specific business types (starting with Marketing Agencies and Media/Publishers),
connects buyers with implementation consultants, and helps SaaS vendors
reach qualified buyers.

TECH STACK:
- Next.js 14 with App Router
- TypeScript
- Tailwind CSS + shadcn/ui components
- Supabase for database and authentication
- OpenAI API for AI Stack Builder feature

CORE PAGES TO BUILD (MVP):

1. Homepage (/)
   - Hero: "Find the perfect SaaS stack for your business"
   - Sub-hero: "We recommend complete technology stacks, not just tools"
   - Featured verticals: Marketing Agency, Media Publisher (more coming)
   - CTA: "Build your stack →" (links to /quiz)
   - Featured comparison strip: 3 comparison cards
   - How it works: 3-step explanation
   - Consultant trust strip

2. Stack Page (/stacks/[slug]) — build template, seed with marketing-agency
   - Hero with stack name + quick stats
   - Visual layer diagram (custom component: StackDiagram)
   - Tool recommendations by layer (ToolCard component)
   - Alternatives table
   - Total cost estimator
   - Consultant CTA section
   - FAQ section with schema markup

3. Comparison Page (/compare/[slug]) — build template, seed with hubspot-vs-salesforce
   - Side-by-side comparison hero
   - Feature comparison table (ComparisonTable component)
   - "When to choose each" cards
   - Affiliate CTAs for both tools
   - Related comparisons

4. Category Page (/categories/[slug]) — build template, seed with crm
   - Hero + definition
   - Quick picks section
   - Tool cards grid (8 tools)
   - Buyer's guide
   - Related stacks

5. AI Stack Builder (/quiz)
   - 5-step multi-page quiz (no page reloads; state in React)
   - Steps: company type → team size → budget → pain points → current tools
   - On submit: call OpenAI API with structured prompt, display stack output
   - Email capture on results page: "Save and email me this stack"
   - CTA: "Find a consultant to implement this"

6. Consultant Directory (/consultants)
   - Filter by: tool specialization, vertical, location, budget
   - ConsultantCard component: avatar, name, company, tools, rating, CTA
   - Lead form on each profile

COMPONENTS TO BUILD:
- ToolCard: logo, name, category, price range, rating, affiliate CTA button
- StackDiagram: visual layered architecture diagram (CSS/SVG)
- ComparisonTable: responsive side-by-side feature matrix
- ConsultantCard: profile card with contact CTA
- QuizFlow: multi-step form with progress indicator
- StackResult: AI output display with tool cards + cost table
- LeadForm: email + message + tool interest capture
- AffiliateButton: tracked CTA with UTM params

DATABASE (Supabase):
Create these tables (use the schema in the attached spec):
- tools
- categories
- verticals
- stacks
- stack_layers
- stack_components
- comparisons
- consultants
- leads
- quiz_sessions

SEED DATA TO CREATE:
- 2 verticals: marketing-agency, media-publisher
- 1 complete stack: Marketing Agency Stack (7 tools across 4 layers)
  Layer 1 Customer: HubSpot (CRM), ActiveCampaign (Email)
  Layer 2 Data: Fivetran (ETL), Snowflake (Warehouse)
  Layer 3 Analytics: Looker (BI), TapClicks (Marketing Analytics)
  Layer 4 Execution: Monday.com (Project Mgmt)
- 5 tools with full data: HubSpot, Salesforce, Klaviyo, Fivetran, Snowflake
- 1 comparison: hubspot-vs-salesforce
- 1 category: crm
- 5 consultant profiles (mock data for now)

SEO REQUIREMENTS:
- generateMetadata() on every dynamic page
- JSON-LD schema: FAQ, Product, BreadcrumbList
- Sitemap: app/sitemap.ts auto-generating from DB
- OpenGraph images: auto-generated with next/og
- All pages use SSG with ISR (revalidate: 3600)

DESIGN:
- Professional, clean SaaS aesthetic
- Color palette: deep navy (#0D1B2A) + electric blue (#2563EB) + white
- Font: Geist (display) + system-ui (body)
- Tool logos: pulled from logo.clearbit.com/{domain}
- Fully responsive (mobile-first)
- Dark mode support

AFFILIATE TRACKING:
- Every tool "Try" button uses: /out/[tool-slug]
- Route /out/[tool-slug] → redirect to affiliate URL + log click to Supabase

START WITH:
1. Project setup (Next.js + Supabase + Tailwind + shadcn)
2. Database schema migration
3. Homepage
4. Marketing Agency Stack page
5. HubSpot vs Salesforce comparison page
6. AI Stack Builder quiz

--- END CURSOR PROMPT ---


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 10 — EXECUTION ROADMAP
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

WEEK 1 — Foundation
  Day 1:  Register domain. Set up Next.js + Supabase + Vercel
  Day 2:  Run DB migrations. Seed tool + stack data
  Day 3:  Build ToolCard + StackDiagram components
  Day 4:  Build Marketing Agency Stack page (full template)
  Day 5:  Build HubSpot vs Salesforce comparison page
  Day 6:  Build CRM category page
  Day 7:  Deploy to Vercel. Test all pages.

WEEK 2 — Core Features
  Day 8:  Build AI Stack Builder quiz (steps 1-5)
  Day 9:  Wire OpenAI API to quiz → stack output
  Day 10: Build lead capture + Supabase lead table
  Day 11: Build consultant directory (mock data)
  Day 12: Build /out/ affiliate redirect route + click logging
  Day 13: Set up Resend email: lead capture confirmation
  Day 14: Set up PostHog analytics

WEEK 3 — Content + SEO
  Day 15: Media Publisher stack page
  Day 16: Klaviyo vs Mailchimp comparison
  Day 17: Fivetran vs Airbyte comparison
  Day 18: Marketing Automation category page
  Day 19: Apply affiliate program to: HubSpot, Monday, Klaviyo, Fivetran
  Day 20: JSON-LD schema on all pages
  Day 21: Submit to Google Search Console. Request indexing.

WEEK 4 — Validation
  Day 22: Post in 3 Slack communities (agency ops groups)
  Day 23: Post on LinkedIn: "I built a tool that recommends stacks for agencies"
  Day 24: DM 10 agency ops directors with the quiz link
  Day 25: Email 5 consultants: "Free listing, here's your profile draft"
  Day 26: Review PostHog session replays. Fix top 3 friction points.
  Day 27: Add 3 more comparison pages based on quiz answer data
  Day 28: Write first blog post targeting long-tail keyword

WEEKS 5-6 — Scale Content
  - Add SaaS Company stack
  - Add eCommerce stack
  - 10 total comparison pages live
  - Start outreach to first vendor sponsor
  - First paid consultant lead (manual)


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART 11 — VALIDATION METRICS (what to track weeks 1-6)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  Signal                    Good (Week 4)   Great (Week 6)   Meaning
  ─────────────────────────────────────────────────────────────────────────
  Quiz completions          10+             50+              Core value validated
  Email captures            5+              30+              People want the output
  Affiliate clicks          20+             100+             Purchase intent confirmed
  Consultant page views     50+             300+             Supply-side interest
  Time on stack pages       > 2 min avg     > 3 min avg      Content quality signal
  Organic impressions (GSC) 100+            1,000+           SEO indexing working
  LinkedIn/community DMs    3+              15+              Word of mouth starting

If quiz completions < 10 at week 4 → problem is awareness, not product.
If quiz completions > 10 but email captures < 2 → problem is output quality.
If email captures > 5 but no affiliate clicks → problem is tool selection/CTAs.


━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
APPENDIX: AFFILIATE PROGRAMS TO JOIN IMMEDIATELY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  Tool            Program URL                           Commission
  ─────────────── ───────────────────────────────────── ──────────────────────
  HubSpot         hubspot.com/partners/affiliates        30% recurring / 1yr
  Monday.com      monday.com/affiliate                   30% first payment
  Klaviyo         klaviyo.com/partners                   20% recurring
  Shopify         shopify.com/affiliates                 $150 per merchant
  Pipedrive       pipedrive.com/partner-program          20% recurring 1yr
  ClickUp         clickup.com/affiliates                 20% recurring
  Fivetran        fivetran.com/partners                  Custom (contact)
  Datadog         datadoghq.com/partner-network          Custom (contact)
  Semrush         semrush.com/partner/affiliates         $200 per sale
  Notion          notion.so/affiliates                   50% first payment

  Join all of these before launch. Approval takes 1-5 days.
  Wire affiliate URLs into Supabase tools table on day 1.


=============================================================================
  END OF SPECIFICATION
  SaaSMatchup.com V1 Build Spec | Generated for Angshuman
=============================================================================
