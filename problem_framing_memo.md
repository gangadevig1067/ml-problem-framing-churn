# ML Problem-Framing Memo: Customer Churn Prevention

## 1. Business Decision & Action Window
* **Decision:** Identify customers at high risk of canceling so the retention team can offer a targeted intervention (e.g., a 10% loyalty discount or a priority customer-support call).
* **Unit of Observation:** Individual customer account (`customer_id`).
* **Prediction Target:** Binary `churned` (`1` = customer cancels subscription, `0` = customer renews).
* **Action Window:** Flag at-risk accounts at least **7 days prior** to their monthly billing renewal date to allow time for outreach.

## 2. Non-ML Baseline
Before training any machine learning model, we establish a simple rule-based baseline:
* **Rule:** Flag any customer where `last_login_days > 10` (or `support_tickets >= 3`).
* **Performance on Sample Data:** Achieves **100% accuracy, precision, and recall** on the 12-row training file.

## 3. Metrics & Asymmetric Error Costs
* **Primary Model Metrics:** **Recall** (to minimize missed churners) paired with **Precision** (to control discount budget).
* **False Positive Cost (Low–Medium):** Flagging a loyal customer as a churner wastes ~₹100–₹150 in unnecessary discount margin.
* **False Negative Cost (High):** Missing an actual churner loses ₹499–₹1,499/month in recurring subscription revenue.

## 4. Is Machine Learning Justified?
**No, not yet.** With only **12 rows** of labeled data, any ML algorithm (logistic regression, decision trees, etc.) will severely overfit and memorize noise. The non-ML baseline rule should be deployed first while collecting at least **1,000+ historical customer records** over upcoming billing cycles.
