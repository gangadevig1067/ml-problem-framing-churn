# Risk Register & Operational Fallback Plan

| Risk | Severity | Potential Harm / Impact | Mitigation Strategy | Fallback Plan |
| :--- | :--- | :--- | :--- | :--- |
| **Small-Sample Overfitting** | High | Rules or models tuned on 12 rows may fail completely on real production traffic. | Freeze ML training until at least 1,000+ historical records are collected. | Use manual weekly customer-success check-ins for `Basic` plan users. |
| **Target Leakage** | High | `last_login_days` or `support_tickets` may reflect activity *after* a user already decided to cancel. | Enforce strict point-in-time feature snapshots 7 days before renewal dates. | Drop `last_login_days` and flag users solely via pre-cancellation `support_tickets >= 3`. |
| **Discount Gaming (False Positives)** | Medium | Loyal users could intentionally stop logging in or open tickets to trigger free discounts. | Limit retention discounts to once every 6 months; offer non-monetary support calls first. | Require team-lead approval before issuing any monetary discount. |
| **Plan-Tier Proxy Bias** | Medium | All 5 churners in the sample are on `Basic` (₹499), risking neglect of `Standard` and `Pro` users. | Track recall and precision separately across every `plan_type` tier. | Send automated monthly satisfaction surveys to all subscription tiers. |
