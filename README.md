# OVERVIEW
This repository contains Big Query code using Google Analytics raw data isolating "organic search traffic" as a lagging indicator to holistic marketing brand investments and **brand demand signals**. 

The data studio dashboard (https://datastudio.google.com/reporting/927c359c-21a7-4635-b33f-50ff30a39afd) brings many of the insights to life.

## THE FRAMEWORK 
Under this framework, **Organic Website Homepage Traffic** is viewed as the **"Little Brother" of Share of Search (SoS)**. Share of Search tracks the total volume of consumer interest in the market, homepage organic traffic captures the direct on-site crystallization of that demand. One cannot scale sustainably without the other.

## Mini Case Study: The Lagged Demand Effect
For an e-commerce brand mapping performance out on a comprehensive KPI tree and pairing **Share of Search (SoS)** with **Organic Homepage Sessions**, we isolated the delayed behavioral correlation between market-level demand generation and final web-level traffic crystallization.

<img width="1002" height="490" alt="image" src="https://github.com/user-attachments/assets/244da0c3-d14c-48c7-8252-399babf53e25" />

### The Macro Scenario:
* **The Brand Campaign (Aug 2023):** A top-of-funnel brand campaign launched in August 2023, causing an immediate, sharp spike in market-level **Share of Search (SoS)**.
* **The Lag Effect:** While the "big brother" metric (SoS) peaked instantly, its "little brother" (Homepage Organic Sessions) remained flat. 
* **The Delayed Activation (Oct 2023):** Roughly 8 to 10 weeks later, the top-of-funnel demand successfully crystallized. Organic homepage traffic experienced a massive spike, dragging baseline recurring performance to a higher tier across the subsequent months.

---

### Share of Search v Organic Homepage Sessions

### The Strategic Takeaways:
By establishing continuous tracking for both **SoS** and **Organic Homepage Sessions** side-by-side, growth teams can actively diagnose and forecast:
1. **The Delayed Brand Lift:** Documenting the immediate top-of-funnel market lift (SoS) to protect brand budgets before web traffic reflects the impact.
2. **The Long-Tail Causation Effect:** Anticipating baseline homepage traffic and downstream organic transaction surges 60–90 days after a major awareness push.

### Understanding Correlation v Causation

#### 1. Correlation
* **The Dynamic:** Simultaneous spikes across macro search interest (Share of Search) and on-site landing page traffic without any chronological delay.
* **The Diagnosis:** This typically indicates **correlation** driven by external macro-market forces (e.g., Black Friday anomalies, holiday peaks, or structural industry-wide shifts) rather than standalone optimization breakthroughs. Both metrics are reacting together to a rising tide.

#### 2. Causation
* **The Dynamic:** Market-wide consumer demand (Share of Search) remains entirely flat, but your on-site `homepage_organic_revenue_share %` scales significantly.
* **The Diagnosis:** This isolated divergence isolates **causation**. Because external market demand did not shift, the upward trend directly proves that internal traffic quality or on-site conversion design mechanics successfully improved—allowing the homepage to capture and convert existing demand more efficiently.

## THE BUSINESS MODEL DIAGNOSTIC MATRIX

| Business Model | Primary Diagnostic Use Case | Key Validation Metrics |
| :--- | :--- | :--- |
| **DTC / Pure E-com (Low AOV)** | Direct performance tracking and frictionless conversion paths. | Web Sessions, Web Revenue, Web Conversion Rate. |
| **DTC / High AOV (Long Cycle)** | Measuring brand equity, top-of-funnel pull, and research behavior. | Web Sessions + **Total Backend Revenue (Shopify)** lagged chronologically. |
| **Multi-Platform (Web + App)** | Verifying web-to-app discovery routing and deep-link health. | Mobile Web Sessions + **App Installs & App Open Rates**. |
| **CPG / Omni-channel Retail** | Media halo effects, offline retail velocity, and store-locator intent. | Web Sessions to utility pages + **Total Operational/Retail Revenue**. |

### FRAMEWORK TO IDENTIFYING FUNNEL TENSION

Every business operates under unique technical frameworks, analytics teams must audit their specific ecosystem to identify which pages are pulling users away from the conversion track:

* **SaaS/Subscription Models:** Focus heavily on isolating the main authentication gateways (`/login`, `/signin`, `/reset-password`). High search volume targeting these keywords implies app authentication loops or broken session cookies.
* **E-Commerce/Retail Engines:** Map out customer service escalations (`/contact-us`, `/returns-refunds`, `/shipping-tracking`). If organic entries jump on these pages, users are likely hunting down delayed packages or struggling to update cart parameters.
* **Lead Generation Platforms:** Monitor structural operational assets (`/help/`, `/faq/`, `/support/`). 

By defining these custom nodes in your SQL case statements, you can clearly track the tension balance in the Data Studio dashboard. If homepage share drops while your custom escalation nodes climb, the data provides an immediate, early warning signal that system friction is actively damaging revenue.

## IDENTIFYING KEY PAGES AND BUILDING THE LOGIC IN THE CODE

### The Operational Solution
The key identifer to be able to track **Organic sessions** is the Landing Page Title (sb.landing_page_title) in the Google Analytics Big Query schema. If two pages or more have the same page title even if they have different URL's it will break the logic. The Landing Page Title has to be unique. 

For example there may be 2 homepages which are being A/B tested - if both homepages **sb.landing_page_title = Homepage** the logic in the code will pick them both. The landing page needs to be as follows: **sb.landing_page_title=Homepage** and **sb.landing_page_title=Home**

To get around handling messy, unpredictable URL extensions and query strings, using **sb.landing_page_title** is the saffest bet. No matter how messy the URL query string gets with ads, campaigns, or tracking UTM's, the title stays exactly the same. It acts as a clean, unified bucket that catches 100% of homepage traffic data in one line of code.

## STRATEGIC PAGE GROUPING ARCHITECTURE & FUNNEL TENSION

To extract actionable brand signals from raw clickstream data, this pipeline splits incoming traffic paths into three distinct structural page groups. This architecture relies on a fundamental concept of **funnel tension**: a direct commercial tug-of-war between high-intent conversion pages and operational troubleshooting hubs.

When system friction increases, user behavior shifts away from commercial targets. Instead of landing on the homepage to explore product lines, users turn to Google or ChatGPT etc, to bypass routes (e.g., searching specifically for a hidden login button, an unlisted support email, or an about page to verify company legitimacy). 

### The Three Tension Pillars

#### 1. The Intent Engine: Homepage Organic Traffic (`homepage_organic_...`)
* **Strategic Role:** The primary collector of pure brand demand signals and user velocity. 
* **Business Impact:** This acts as the direct transactional gateway for premium product catalogs. Higher volume indicates clean, frictionless consumer intent.

#### 2. The Consideration Layer: About & Content Pages (`about_organic_...`)
* **Strategic Role:** Captures mid-funnel users researching brand authority and legitimacy.
* **Business Impact:** While essential for building long-term trust, an unexpected spike in users landing directly here via search can indicate that your primary acquisition journey is confusing, forcing users to manually investigate who you are before buying.

#### 3. The Friction Node: Escalation & Login Hubs (`escalation_total_sessions`)
* **Strategic Role:** Aggregates entries directly onto account portals, contact forms, help docs, and support centers.
* **Business Impact:** This represents your **revenue drainage metric**. High traffic share here proves that users are utilizing search engines as a diagnostic tool to fix broken experiences. When this bucket balloons, it introduces negative friction that starves the homepage intent engine and suppresses conversion rates.

## THE CORE ANALYTICAL GUARDRAILS 

### 1. The Mobile App & Omni-channel "Black Hole"
* **The Nuance:** For brands with a native iOS/Android apps, standard web-to-revenue correlation will naturally skew. High-intent users searching the brand organically on mobile web will frequently be deep-linked directly out of the browser and into the native app ecosystem to complete their purchase. 
* **The Strategy:** Do not interpret a spike in mobile organic homepage traffic paired with flat *web* revenue as a commercial failure. Use the weekly cohort baseline (`week_start_monday`) to blend data silos. Validate the traffic spike by checking for a synchronized, chronological lift across native **Mobile App Installs / App Open Velocity** alongside total backend revenue.

### 2. High-AOV & Non-Linear Web Journeys (The Delayed Conversion Halo)
* **The Nuance:** For pure e-commerce brands where transactions happen *entirely* on the website, user journeys are still heavily impacted by product type and consideration windows. High-AOV products inherently trigger non-linear behavior (Search > Home > Abandon > Research > Direct Return Days Later). 
* **The Strategy:** Treat the homepage as a baseline demand-generation engine. If you observe a massive spike in weekly organic homepage sessions, look past immediate session-level web revenue. Instead, monitor **Total Storewide Revenue** (e.g., pulled directly from Shopify/ERP) over a lagged 2-to-4 week window. Use Desktop Organic traffic trends as your baseline control group to evaluate pure user intent, as desktop users exhibit lower friction and longer session dwell times.

### 3. Framework Scale Variance (The Macro vs. Micro Reality)
* **The Nuance:** The close alignment of marketing discovery metrics and backend sales depends heavily on business scale. 
  * **At Lower Volume (~£2M AUM):** Data loops are tight and channel crossover is minimal, meaning homepage organic trends often track tightly and directly with total business revenue.
  * **At Scale (£10M+ AUM):** The relationship transitions into a macro trend-line compass rather than a literal transactional map. App-deep linking, multi-device paths, and complex attribution leakage will naturally separate immediate session web revenue from total performance.
* **The Strategy:** Evaluate the relationship holistically as an ecosystem health signal. Continuous, historic visualization of these metrics side-by-side remains essential: sudden, severe structural divergences between traffic share and backend revenue act as an early-warning diagnostic tool for technical tracking failures, indexation bugs, or broken cross-platform customer journeys.

# CONTEXTUAL NUANCE & CHECKLIST 

This dashboard is built for for **Anomaly Detection and Trend Analysis**. The data models are behavioral frameworks rather than absolute physics, these charts must be interpreted alongside broader cross-channel marketing and operational contexts. 

The 4 step checklist when evaluating major shifts or spikes in the data view:

### 1. The Cross-Channel Spillover Caveat
* **Paid Media / Email Alignment:** If aggressive paid media (Google Brand Ads, Meta Ads) or mass email marketing campaigns are directed straight to the homepage, this view loses its isolated "organic purity."
* **The Halo Effect:** A heavy paid or email push naturally drives a large percentage of users to the homepage will make it harder to isolate the organic brand flywheel.

### 2. The Operational Anomaly Caveat (The "PR Typo" Dynamics)
* **High-Revenue Utility Spikes:** A sudden surge in traffic to an escalation, registration, or login page is not automatically a sign of a broken journey. 
* **The Behind-the-Scenes Reality:** This shift can be caused by positive anomalies, such as a viral PR campaign an offline event featuring a misspelled vanity URL, or a massive product drop requiring rapid account creation. 
* **Rule of Thumb:** The stacked bar chart flags *where* the structural shift happened; human data exploration is required to diagnose *why*.

### 3. The Business Model & Lifecycle Adjuster
* **Audience Composition Nuance:** The baseline ratio between **Homepage Traffic** and **Tension Pages** is entirely unique to a brand's current lifecycle maturity.
* **The Scale Shift:** A young, scaling DTC brand should expect an organic footprint heavily dominated by the homepage. A mature subscription brand with a bigger base of subscribers will naturally see a higher, permanent baseline of tension from utility login pages—and that is completely healthy.

### 4. The Native Mobile App "Black Hole" Caveat
* **The Journey Shatter:** For brands with a native iOS/Android app, standard web-to-revenue correlation will naturally skew. 
* **Deep-Linking Disruption:** High-intent users searching the brand organically on mobile web will frequently be deep-linked directly out of the browser and into the native app ecosystem to complete their purchase. 
* **The Analytical Guardrail (Ecosystem Validation):** A spike in mobile organic homepage traffic accompanied by flat *web* revenue must not be interpreted as a poor commercial journey. To validate the macro flywheel, analysts must stop looking at web revenue in isolation. 
* **The Integration Strategy:** You must bypass web-attribution tracking limits by using the weekly cohort baseline (`week_start_monday`) to blend data silos. Validate the organic traffic spike by checking for a synchronized, chronological lift across:
  1. **Total Ecosystem Revenue** (pulled directly from your source of truth, e.g., Shopify).
  2. **Mobile App Installs / App Open Velocity** (pulled from Apple App Store Connect & Google Play Console).
* **The Bottom Line:** If total Shopify revenue and app metrics climb while web-specific conversions remain flat during a homepage traffic spike, it proves your search visibility is successfully feeding and activating the wider ecosystem.

# DATE SORTING IN DATA STUDIO 

When passing text strings like `Jan-2024`,`Feb-2025` Data Studio defaults to sorting alphabetical characters (grouping all `Apr` months together, then all `Aug` months), breaking the multi-year timeline sequence.

### The Fix
To force clean chronological bar graph arrays without modifying your backend BigQuery storage layer tables, add a custom calculated field directly within your individual chart components named **`Month-Year Order`** using the explicit formula block below:

```text
CASE
  WHEN month_year = 'Jan-2024' THEN 1
  WHEN month_year = 'Feb-2024' THEN 2
  WHEN month_year = 'Mar-2024' THEN 3
  WHEN month_year = 'Apr-2024' THEN 4
  WHEN month_year = 'May-2024' THEN 5
  WHEN month_year = 'Jun-2024' THEN 6
  WHEN month_year = 'Jul-2024' THEN 7
  WHEN month_year = 'Aug-2024' THEN 8
  WHEN month_year = 'Sep-2024' THEN 9
  WHEN month_year = 'Oct-2024' THEN 10
  WHEN month_year = 'Nov-2024' THEN 11
  WHEN month_year = 'Dec-2024' THEN 12
  WHEN month_year = 'Jan-2025' THEN 13
  WHEN month_year = 'Feb-2025' THEN 14
  WHEN month_year = 'Mar-2025' THEN 15
  WHEN month_year = 'Apr-2025' THEN 16
  WHEN month_year = 'May-2025' THEN 17
  WHEN month_year = 'Jun-2025' THEN 18
  WHEN month_year = 'Jul-2025' THEN 19
  WHEN month_year = 'Aug-2025' THEN 20
  WHEN month_year = 'Sep-2025' THEN 21
  WHEN month_year = 'Oct-2025' THEN 22
  WHEN month_year = 'Nov-2025' THEN 23
  WHEN month_year = 'Dec-2025' THEN 24
  WHEN month_year = 'Jan-2026' THEN 25
  WHEN month_year = 'Feb-2026' THEN 26
  WHEN month_year = 'Mar-2026' THEN 27
  WHEN month_year = 'Apr-2026' THEN 28
  WHEN month_year = 'May-2026' THEN 29
  WHEN month_year = 'Jun-2026' THEN 30
  ELSE 99
END
