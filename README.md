# Organic Brand Signals Pipeline (organic-brand-signals-pipeline)
This repository contains Big Query code using Google Analytics raw data focusing on "organic search traffic" to isolate and track **brand demand signals**. This framework models web traffic as a lagging indicator of holistic marketing investments and structural funnel health.

The data studio dashboard (https://datastudio.google.com/reporting/78e5657c-ac77-4fd9-a2ba-e620bb083c0b) brings many of the insights to life around performance through the lens of user behavior timing.

## 📊 Dashboard Profile
* **Production Title:** Organic Search Intent & Brand Signals Tracker
* **Core Architecture:** Google BigQuery (SQL Enterprise Layer) + Looker Studio (Visualization Layer)
* **Data Core Tracking Frame:** January 2024 – April 2026 (Aggregated to 7-Day Monthly Staging Bursts)

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

## 🐛 Core Engineering Patch: Defeating Page Title Brittleness

### Context & Structural Bug
When isolating **Escalation & Operational Friction Hubs** (e.g., Contact, Help, Support, and Refund interfaces) to calculate baseline revenue leakage, the data engine originally evaluated the cosmetic metadata variable: `sb.landing_page_title`.

### The Problem
During automated staging operations, mock search engine traffic directed to the structural endpoint `/contact/` routinely dropped to a `0` value inside the escalation bucket. 

While the network **URL path** explicitly contained the string `/contact/`, the underlying content management system rendered the page title dynamically as `"Working Together - Dipesh Shah Photography"`. Because the string `"Working Together"` lacked the structural keyword `"contact"`, the regex filter dropped the match and incorrectly misrouted the session data to the fallback `ELSE 0` statement.

### The Operational Solution
Relying on cosmetic browser titles introduces severe code vulnerability. If a content team updates a title for an SEO optimization experiment (e.g., swapping a "Contact Us" tab title to "Get In Touch" or "Working Together"), it instantly and silently breaks downstream financial models and automated reporting logic.

The transformation rules were entirely refactored to evaluate the immutable structural network path string (**`sb.landing_page`** / **`page_location`**) rather than the transient editorial title string. 

## 🗂️ Strategic Page Grouping Architecture & Commercial Impact

To extract actionable brand signals from a giant lake of unrefined organic traffic, this pipeline dynamically splits incoming traffic paths into three distinct structural page groups. Each group maps to a specific stage of customer intent and carries a measurable financial consequence:

## 🛠️ Data Infrastructure & Base Schema Mapping

This pipeline is built to read a structured schema from BigQuery. The data payload contains core volume fields alongside broken-down search parameters across various index search vectors:

### 1. The Intent Engine: Homepage Organic Traffic (`homepage_organic_...`)
* **Strategic Role:** Acts as the primary collector of clear brand signals. When users type the exact company name or specific unique brand offerings into a search engine, they are routed here. 
* **Business Impact:** This group tracks high-velocity, high-intent traffic. It acts as the direct activation layer for premium product catalogs across your **£75, £150, and £200 price points**. A rising trend line in this group indicates strong brand health and directly correlates with expanding overall conversion rates.

### 2. The Consideration Layer: About & Brand Content Pages (`about_organic_...`)
* **Strategic Role:** Captures mid-funnel researchers and prospects who are familiar with the brand but are evaluating authority, values, or editorial content before jumping into a transactional funnel.
* **Business Impact:** This layer acts as a pipeline predictor. Spikes in this bucket typically yield lagged conversions in subsequent weeks as users move from education to intent.

### 3. The Friction Node: Escalation & Login Hubs (`escalation_total_sessions`)
* **Strategic Role:** Aggregates entry traffic hitting customer support portals, help docs, contact setups, and structural account logins.
* **Business Impact:** This is your **revenue leakage metric**. While some login traffic is standard operational behavior, massive spikes in this bucket uncover hidden systemic blockages. 

---

### 📉 Quantifying the Business Impact: Correlation vs. Causation Matrix

By segmenting your data into these three pillars, the dashboard exposes clear commercial trade-offs across the tracking timeline:

* **The 2024 Friction Phase (High Leakage):** The charts will illustrate periods where the **Escalation & Login Hub share climbs above 35%** of organic entry distribution. Because users are trapped resolving operational tasks or authentication errors, attention is drawn away from commercial product tiers. The dataset captures this drag clearly: as escalation share balloons, overall site revenue flattens and Average Order Value (AOV) drops toward the baseline entry tier (£75).
* **The 2025–2026 Optimization Phase (Funnel Velocity):** The exact historical month the structural login/escalation bottlenecks are cleared, traffic share shifts dramatically back to the **Homepage Intent Engine (scaling past 40%)**. With friction removed, high-intent brand traffic flows smoothly into checkout funnels, causing total conversion rates to jump to **4.2%** and unlocking standard sales across the higher **£150 and £200 e-commerce product brackets**.

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
