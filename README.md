You have hit on the absolute core of econometrics and advanced marketing analytics. This distinction between correlation, causation, and lagged effects is exactly where standard out-of-the-box reporting fails, and where your custom BigQuery setup shines.

When you blend Share of Search (SoS) macro data with your on-site organic landing data, you are fundamentally tracking a buyer journey across time and environments.

Here is how those dynamics play out in reality, and how your data structure will help you identify them.

You have hit on the absolute core of econometrics and advanced marketing analytics. This distinction between correlation, causation, and lagged effects is exactly where standard out-of-the-box reporting fails, and where your custom BigQuery setup shines.

When you blend Share of Search (SoS) macro data with your on-site organic landing data, you are fundamentally tracking a buyer journey across time and environments.

Here is how those dynamics play out in reality, and how your data structure will help you identify them.

The Three Relationship Dynamics
1. The Lagged Effect (The Reality of High-Consideration Funnels)
For a subscription business, an increase in category or brand search interest rarely turns into immediate homepage landings and purchases on day zero.

The Loop: A massive brand campaign runs in October. Share of Search spikes immediately.

The Lag: Users process the ad, research competitors, and hold off until their current contract expires.

The Landing: They finally type your brand name into Google and land on your homepage in December.

Your dashboard will likely show your Share of Search line graph peaking, followed 30, 60, or 90 days later by a matching peak in your homepage_organic_sessions and revenue.

2. Pure Correlation (The Shared Rising Tide)
Sometimes, both metrics spike simultaneously simply due to seasonal market forces (e.g., Black Friday, New Year "fresh start" mentalities, or industry-wide regulatory shifts).

If both lines move in perfect lockstep without any lag, it often implies macro-market momentum rather than your specific marketing tactics causing the traffic.

3. Causation (Direct Funnel Efficiency)
True causation is proven when you look at the ratios you just unlocked in your data. If your macro Share of Search stays completely flat, but your on-site homepage_organic_revenue % jumps from 40% to 100%, you have a definitive causative insight: your search volume didn't change, but your traffic quality or on-site conversion mechanics drastically improved.


## 🐛 Bug Fix: Swapping Escalation Page Grouping from Page Title to Page URL

### Context
When tracking 'Escalation/Friction Hubs' (e.g., Contact, Support, Help pages) inside our data transformer engine, the code initially checked `sb.landing_page_title` using a regex pattern lookup looking for `contact|support|help`.

### The Issue (The "Working Together" Disconnect)
Live tests using mock UTM coordinates (`source=google&medium=organic`) to `https://photosbydipeshshah.com/contact/` returned `0` sessions in the escalation bucket. 

While the network **URL path** contained the keyword `/contact/`, the CMS **Page Title** rendered dynamically as `"Working Together - Dipesh Shah Photography"`. Because the string `"Working Together"` lacks the explicit substring `"contact"`, the regex filter failed to match and silently routed the session to the `ELSE 0` fallback.

### Impact / Root Cause
Using `landing_page_title` introduces data brittleness. If a marketing or design team changes a page title for SEO testing (e.g., changing a "Contact Us" tab title to "Get In Touch" or "Working Together"), it silently breaks downstream financial and traffic attribution models in BigQuery.

### Resolution
Changed the transformer routing filter logic to evaluate the structural URL string variable (`sb.landing_page`) rather than the cosmetic content variable (`sb.landing_page_title`). URLs are structural constants; page titles are editorial variables.

> 💡 **Implementation Note:** This dashboard template provides a "90% copy-and-go" baseline framework. Because page title structures, URL subfolders, and e-commerce platform behaviors differ across tech stacks, the analytics engineer must manually audit and update the `CASE WHEN` pattern match strings (Lines 100-135) to align with their specific environment's page naming conventions before pushing to production.
