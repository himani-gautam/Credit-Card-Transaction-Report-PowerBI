
Power BI dashboard analyzing credit card transactions, revenue growth, and customer insights.
# Credit Card Transaction & Customer Analysis Dashboard

**One-line:** A Power BI dashboard that analyzes credit card transactions and customer behavior to surface business KPIs like revenue trends, churn, delinquency, and acquisition cost.

---

## Table of Contents

1. Overview
2. Dataset
3. Key Features
4. KPIs & Metrics
5. Screenshots / Demo
6. How to open (Quick Start)
7. Files in this repository
8. Technical details (DAX + transformations)
9. Insights & Recommendations
10. Future improvements
11. Contact

---

## 1. Overview

This dashboard provides an at-a-glance view of credit card business health. It helps stakeholders quickly understand revenue trends, customer segments, payment behavior, delinquency patterns, and churn indicators so they can prioritize retention and risk-mitigation actions.

---

## 2. Dataset

Credit card transaction data.

---

## 3. Key Features

* Revenue trend (Daily / Monthly / YoY)
* Customer segmentation (age, income, region)
* Delinquency & default indicators
* Churn analysis and customer lifecycle
* Running totals & moving averages for smoothing
* Interactive filters: date, region, customer segment, payment status

---

## 4. KPIs & Metrics (Displayed)

* **Total Revenue** — sum of transaction amounts
* **Monthly Active Customers (MAC)** — count of customers with at least one transaction in month
* **Customer Acquisition Cost (CAC)** — (if marketing cost data available)
* **Churn Rate (%)** — percentage of customers who stopped transacting
* **Delinquency Rate (%)** — % of customers with late or missed payments
* **Average Transaction Value (ATV)**

---

## 5. Screenshots / Demo
https://github.com/himani-gautam/Credit-Card-Transaction-Report-PowerBI/blob/main/dash%20credit%20card.png


*Caption:* Credit Card Dashboard overview showing KPIs, revenue trend, and delinquency heatmap.

---

## 6. How to open 

**If you want to open it on GitHub (web):**

* The screenshot above gives a preview. To interact with the report online.
---

## 7. Files in this repository

* `README.md` — this file
* `assets/credit_card_dashboard_screenshot.png` — dashboard screenshot
* `files/Credit_Card_Dashboard.pbix` — Power BI file (if included)
* `data/sample_transactions.csv` — (optional) sample dataset used for demo
* `scripts/` — (optional) SQL or preprocessing scripts

---

## 8. Technical details (DAX & Transformations)

### Data prep / transformations

* Merge transaction and customer master tables on `CustomerID`.
* Create calendar table and mark as Date table.
* Fill missing income/age with median where appropriate.
* Bucket customers into income/age segments for segmentation visuals.

### Example DAX measures (copy & paste into Power BI):

**Cumulative Revenue**

```dax
Cumulative Revenue =
CALCULATE(
    SUM(Transactions[Amount]),
    FILTER(
        ALLSELECTED('Calendar'[Date]),
        'Calendar'[Date] <= MAX('Calendar'[Date])
    )
)
```

**Month-over-Month Revenue %**

```dax
MoM Revenue % =
VAR Prev = CALCULATE(SUM(Transactions[Amount]), PREVIOUSMONTH('Calendar'[Date]))
VAR Curr = SUM(Transactions[Amount])
RETURN DIVIDE(Curr - Prev, Prev, 0)
```

**Churn Rate (example)**

```dax
Churn Rate (%) =
VAR TotalCust = DISTINCTCOUNT(Customers[CustomerID])
VAR Churned = CALCULATE(DISTINCTCOUNT(Customers[CustomerID]), FILTER(ALL(Customers), Customers[Status] = "Churned"))
RETURN DIVIDE(Churned, TotalCust, 0)
```

**Delinquency Rate**

```dax
Delinquency Rate (%) =
DIVIDE(
    CALCULATE(DISTINCTCOUNT(Transactions[CustomerID]), Transactions[PaymentStatus] = "Late"),
    DISTINCTCOUNT(Transactions[CustomerID]),
    0
)
```

---

## 9. Insights & Recommendations (example)

* **Insight 1:** 60% of revenue comes from customers aged 30–45 — target retention campaigns here.
* **Insight 2:** Delinquency rate spikes in a specific region — investigate local policies or economic conditions.
* **Insight 3:** New customer acquisition is high but CAC is increasing — optimize marketing channels.

**Business recommendations:**

* Launch targeted offers for high-value age groups.
* Strengthen credit-monitoring for high-delinquency segments.
* Implement early-warning alerts for accounts showing late payment patterns.

---

## 10. Future improvements

* Add a predictive credit risk scoring model (ML) that flags at-risk customers.
* Automate data refresh using Power BI Gateway and scheduled refresh.
* Add cohort analysis and LTV forecasting.

---

## 11. Contact

**Author:** Himani Gautam 

* LinkedIn: www.linkedin.com/in/himanigautam

---



