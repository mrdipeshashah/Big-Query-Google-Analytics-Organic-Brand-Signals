# OVERVIEW
This repository contains Big Query code using Google Analytics raw data isolating "organic search traffic" as a lagging indicator to holistic marketing brand investments and **brand demand signals**. 

The data studio dashboard (https://datastudio.google.com/reporting/927c359c-21a7-4635-b33f-50ff30a39afd) brings many of the insights to life.

## THE FRAMEWORK 
Under this framework, **Organic Website Homepage Traffic** is viewed as the **"Little Brother" of Share of Search (SoS)**. Share of Search tracks the total volume of consumer interest in the market, homepage organic traffic captures the direct on-site crystallization of that demand. One cannot scale sustainably without the other.

### Mini Case Study - The Lagged Effect
For an e-commerce business mapping out KPI's. developed a KPI tree and drilled into understanding the impact of brand through the eyes of **Organic Homepage sessions**. There was a disconnect between brand and 'performance' in how the data was vieweed, the first step was building a view of the available data in isolating **Organic Homepage Sessions** then added in **Sos** data showing the lag between brand impact with **Sos** and traffic / conversion.

**SoS** data or **Organic Homepage Sessions** were not being tracked. There was a brand tracker in place.  

For an e-commerce business: 
* **The Brand Campaign:** A brand campaign runs in **August-2023**, causing an immediate spike in **Share of Search (SoS)**.
* **The Lag Affect:** The "big brother" **SoS** peaked immediately, but the "little brother" **Homepage Organic Sessions** remained flat
* **The Action:** 8 to 10 weeks later in **October-2023**, there was an increase in **Organic homepage sessions** that drove to purchases.

<img width="1002" height="490" alt="image" src="https://github.com/user-attachments/assets/244da0c3-d14c-48c7-8252-399babf53e25" />
(this data was looked at in 2024) 

If both **Sos** and **Organic Homepage Sessions** were being regularly tracked it would be able to pick up on: 

1. The increase in brand searches / brand lift (**Sos**) from August 2023 and the baseline increasing in the following months 
2. The causation affect on **Organic Homepage Sessions** from October 2023 and the baseline increasing in the following months 

### Understanding correlaton v causation 

### 1. Correlation
Simultaneous spikes across macro search interest and on-site landing pages without any chronological delay typically suggest seasonal macro-market forces (e.g., Black Friday anomalies or structural industry-wide shifts) rather than standalone channel optimization success.

### 2. Causation
Causation is isolated through on-site conversion rate. If market demand (Share of Search) stays entirely flat, but your on-site `homepage_organic_revenue_share %` scales, you have proven definitive causation as market demand didn't shift, but your internal traffic quality or conversion design mechanics successfully improved, allowing the "little brother" to convert more efficiently.

## THE BUSINESS MODEL DIAGNOSTIC MATRIX

| Business Model | Primary Diagnostic Use Case | Key Validation Metrics |
| :--- | :--- | :--- |
| **DTC / Pure E-com (Low AOV)** | Direct performance tracking and frictionless conversion paths. | Web Sessions, Web Revenue, Web Conversion Rate. |
| **DTC / High AOV (Long Cycle)** | Measuring brand equity, top-of-funnel pull, and research behavior. | Web Sessions + **Total Backend Revenue (Shopify)** lagged chronologically. |
| **Multi-Platform (Web + App)** | Verifying web-to-app discovery routing and deep-link health. | Mobile Web Sessions + **App Installs & App Open Rates**. |
| **CPG / Omni-channel Retail** | Media halo effects, offline retail velocity, and store-locator intent. | Web Sessions to utility pages + **Total Operational/Retail Revenue**. |

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

---

### FRAMEWORK TO IDENTIFYING FUNNEL TENSION

Every business operates under unique technical frameworks, analytics teams must audit their specific ecosystem to identify which pages are pulling users away from the conversion track:

* **SaaS/Subscription Models:** Focus heavily on isolating the main authentication gateways (`/login`, `/signin`, `/reset-password`). High search volume targeting these keywords implies app authentication loops or broken session cookies.
* **E-Commerce/Retail Engines:** Map out customer service escalations (`/contact-us`, `/returns-refunds`, `/shipping-tracking`). If organic entries jump on these pages, users are likely hunting down delayed packages or struggling to update cart parameters.
* **Lead Generation Platforms:** Monitor structural operational assets (`/help/`, `/faq/`, `/support/`). 

By defining these custom nodes in your SQL case statements, you can clearly track the tension balance in the Data Studio dashboard. If homepage share drops while your custom escalation nodes climb, the data provides an immediate, early warning signal that system friction is actively damaging revenue.

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
