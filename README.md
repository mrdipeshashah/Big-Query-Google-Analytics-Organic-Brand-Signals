# Organic Brand Signals Pipeline (organic-brand-signals-pipeline)

An advanced SQL data transformation and pipeline framework engineered for BigQuery and Looker Studio. This project deconstructs generic "organic search traffic" to isolate and track true **brand demand signals**, mapping customer velocity against high-value commercial paths and operational friction hubs.

Instead of relying on out-of-the-box attribution models, this framework models web traffic as a lagging indicator of holistic marketing investments and structural funnel health.

---

## 📊 Dashboard Profile
* **Production Title:** Organic Search Intent & Brand Signals Tracker
* **Core Architecture:** Google BigQuery (SQL Enterprise Layer) + Looker Studio (Visualization Layer)
* **Data Core Tracking Frame:** January 2024 – April 2026 (Aggregated to 7-Day Monthly Staging Bursts)

---

## 📈 The Core Econometric Philosophy

A common mistake in digital analytics is treating organic traffic as a "marketing-free layer" that generates its own customer demand. In reality, search volume behaves as a trailing echo of prior brand investments, offline exposure, and upstream campaign momentum. 

This model isolates three distinct relationship dynamics within your search data:

### 1. The Lagged Effect (Trailing Brand Yield)
In premium e-commerce tiers (£75, £150, and £200 product lines), top-of-funnel marketing campaigns rarely spark day-zero conversions. 
* **The Cycle:** A high-impact brand campaign runs in **October**, causing an immediate spike in macro category Search interest.
* **The Lag:** Users process the messaging, research alternatives, and evaluate their budgets over a 30, 60, or 90-day window.
* **The Action:** The customer returns in **December** via a targeted branded search engine query, landing directly on your homepage to purchase.
* **Pipeline Application:** The timeline line charts inside this tracker are configured to pinpoint these offset waves, validating the delayed economic return of brand building.

### 2. Pure Correlation (The Rising Tide)
Simultaneous spikes across all search entrance pages without any chronological delay typically suggest seasonal macro-market forces (e.g., Black Friday anomalies or structural industry-wide shifts) rather than standalone channel optimization success.

### 3. True Causation (Funnel Efficiency Activation)
Causation is isolated through on-site data ratios. If macro market demand stays entirely flat, but your on-site `homepage_organic_revenue_share %` scales, you have proven definitive structural causation: traffic composition and on-site checkout mechanics successfully improved.

---

## 🐛 Core Engineering Patch: Defeating Page Title Brittleness

### Context & Structural Bug
When isolating **Escalation & Operational Friction Hubs** (e.g., Contact, Help, Support, and Refund interfaces) to calculate baseline revenue leakage, the data engine originally evaluated the cosmetic metadata variable: `sb.landing_page_title`.

### The Problem
During automated staging operations, mock search engine traffic directed to the structural endpoint `/contact/` routinely dropped to a `0` value inside the escalation bucket. 

While the network **URL path** explicitly contained the string `/contact/`, the underlying content management system rendered the page title dynamically as `"Working Together - Dipesh Shah Photography"`. Because the string `"Working Together"` lacked the structural keyword `"contact"`, the regex filter dropped the match and incorrectly misrouted the session data to the fallback `ELSE 0` statement.

### The Operational Solution
Relying on cosmetic browser titles introduces severe code vulnerability. If a content team updates a title for an SEO optimization experiment (e.g., swapping a "Contact Us" tab title to "Get In Touch" or "Working Together"), it instantly and silently breaks downstream financial models and automated reporting logic.

The transformation rules were entirely refactored to evaluate the immutable structural network path string (**`sb.landing_page`** / **`page_location`**) rather than the transient editorial title string. 

---

## 🛠️ Data Infrastructure & Base Schema Mapping

This pipeline is built to read a structured schema from BigQuery. The data payload contains core volume fields alongside broken-down search parameters across various index search vectors:

| Field Name | Type | Description |
| :--- | :--- | :--- |
| `date` | DATE | Core chronological tracking anchor |
| `month_year` | STRING | Text format month label (Format: `Jan-2024`) |
| `total_site_sessions` | INTEGER | High-level overall site entry traffic volume |
| `total_site_revenue` | FLOAT | Combined gross site revenue metrics |
| `homepage_organic_google_sessions` | INTEGER | Main organic brand demand volume driver |
| `homepage_organic_google_revenue` | FLOAT | Core conversion performance for £75, £150, £200 product categories |
| `escalation_total_sessions` | INTEGER | Combined volume tracking across friction/login nodes |

> 💡 **Implementation Note:** This dashboard template provides a "90% copy-and-go" baseline framework. Because page title arrays, subfolder structures, and platform parameters vary widely across e-commerce tech stacks, the implementing analytics engineer must finalize the last 10% configuration by manually auditing and updating the explicit `LIKE` string definitions (Lines 100-135) to match their live network nodes.

---

## 📊 Dashboard Visualizations & Looker Studio Sort Patch

When passing text strings like `Jan-2024` or `Feb-2025` into a visualization front-end, Looker Studio defaults to sorting alphabetical characters (grouping all `Apr` months together, then all `Aug` months), breaking the multi-year timeline sequence.

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
  ELSE 99
END
