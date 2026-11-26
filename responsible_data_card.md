# Responsible Data Card: Customer Churn Training Dataset

## 1. Dataset Overview & Provenance
* **Dataset Name:** `customer_churn.csv`
* **Source / Provenance:** Sample labeled tabular dataset provided for responsible ML baseline framing and evaluation.
* **Size & Structure:** 12 customer records (rows) and 7 columns (`customer_id`, `tenure_months`, `support_tickets`, `monthly_spend_inr`, `last_login_days`, `plan_type`, `churned`).
* **Currency & Region Context:** Subscription billing amounts (`monthly_spend_inr`) are denominated in Indian Rupees (INR).

## 2. Intended Use & Limitations
* **Intended Use:** Benchmarking a simple non-ML retention rule and auditing data quality/risks prior to collecting production data.
* **Out-of-Scope / Prohibited Use:** **Not** suitable for training production ML classifiers (due to extreme small-sample size) or making automated account suspension/pricing decisions.

## 3. Consent, Privacy & Identifiers
* **Identifiers:** Contains pseudonymous account IDs (`C001`–`C012`). No direct PII (names, emails, phone numbers) is included in this table.
* **Consent Requirements:** Before scaling to real user data, product terms of service and privacy notices must explicitly cover analyzing login timestamps and support ticket frequency for customer retention outreach.

## 4. Sensitive Attributes & Proxy Bias
* **Direct Sensitive Attributes:** None present (no age, gender, caste, religion, or location columns).
* **Proxy Attributes & Skew:** `plan_type` (`Basic`, `Standard`, `Pro`) and `monthly_spend_inr` (₹499–₹1,499) act as socioeconomic/income proxies.
* **Observed Skew:** **100% of churned accounts (5 out of 5)** in this dataset belong to the lowest-cost `Basic` tier (₹499/month). Relying blindly on this pattern risks under-serving or failing to detect churn among `Standard` and `Pro` subscribers.

## 5. Missingness & Class Distribution
* **Missing Values:** **0 missing values** (100% completeness across all 12 rows and 7 columns).
* **Target Balance (`churned`):** 5 positive cases (`1` = 41.7%) and 7 negative cases (`0` = 58.3%).

## 6. Data Leakage Risks
* **Post-Cancellation Inactivity (`last_login_days`):** High leakage risk if `last_login_days` includes days *after* a user already canceled or disabled their account rather than strictly measuring inactivity *before* the cancellation decision.
* **Cancellation Tickets (`support_tickets`):** High leakage risk if support tickets include "please cancel my subscription" requests rather than pre-churn product issues.
* **Mitigation:** All feature values must be frozen using point-in-time snapshots taken at least **7 days prior** to the billing renewal date.
