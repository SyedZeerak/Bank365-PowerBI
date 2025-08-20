# <div align="center">Power BI – Bank Transaction Analytics Dashboard</div>

<p align="center">
  <img src="images/preview.gif" alt="Power BI Dashboard Preview" width="880">
</p>

<p align="center">
  &nbsp;
  <a href="Bank365.pbix" target="_blank" style="text-decoration:none;">
    <img alt="Download PBIX" src="https://img.shields.io/badge/Download-.pbix-1a73e8?logo=microsoftpowerpoint&logoColor=white&labelColor=1a1a1a">
  </a>
  &nbsp;
  <a href="#-connect" style="text-decoration:none;">
    <img alt="Contact" src="https://img.shields.io/badge/Contact-📝-6a5acd?labelColor=1a1a1a">
  </a>
</p>

---

## ✨ What this repo contains

An end‑to‑end Power BI project that turns raw card‑transaction data into a concise executive story: trends, customer behavior, merchant performance, churn/retention, geography, fraud and chip success.

* **Power BI file:** `Bank365.pbix`
* **Screens & assets:** `images/*`
* **Documentation:** this interactive README with DAX and modeling notes

---

## 🧭 Quick tour

<details>
<summary><b>1) Dataset Overview</b> — click to expand</summary>

* **4 core tables:** `Transactions` (fact), `Users`, `Cards`, `Merchants`
* Prepped in **Power Query** (source was already fairly clean)
* **Custom columns** for richer analysis:

  * Age Group
  * Income Level
  * Last Transaction Date
  * Active vs Inactive Users
  * Time of Day (transaction buckets)

</details>

<details>
<summary><b>2) Star Schema & Relationships</b> — click to expand</summary>

* **Fact:** `Transactions`
* **Dimensions:** `Users`, `Cards`, `Merchants`
* One‑to‑many relationships from dimensions to fact for performant, filter‑direction‑aware reporting.
* See: `images/data_model.png`

</details>

<details>
<summary><b>3) Measures (30+ DAX)</b> — click to expand</summary>

```DAX
-- Total Transaction Value
Total Txn Value :=
SUM ( Transactions[Amount] )

-- Fraud Rate (based on error types)
Fraud Rate :=
DIVIDE (
    CALCULATE ( COUNTROWS ( Transactions ), Transactions[ErrorType] IN { "Fraud", "Stolen", "Lost" } ),
    COUNTROWS ( Transactions )
)

-- Chip Success Rate
Chip Success Rate :=
DIVIDE (
    CALCULATE ( COUNTROWS ( Transactions ), Transactions[ChipResult] = "Success" ),
    CALCULATE ( COUNTROWS ( Transactions ), NOT ISBLANK ( Transactions[ChipResult] ) )
)

-- Customer Lifetime Value (example)
CLV :=
VAR NetPerSession :=
    AVERAGEX (
        FILTER ( 'Transactions', 'Transactions'[Status] = "Occurred" ),
        'Transactions'[NetRevenue]
    )
VAR SessionsPerCustomer :=
    DIVIDE (
        CALCULATE ( COUNTROWS ( 'Transactions' ), 'Transactions'[Status] = "Occurred" ),
        CALCULATE ( DISTINCTCOUNT ( 'Transactions'[UserID] ), 'Transactions'[Status] = "Occurred" )
    )
RETURN NetPerSession * SessionsPerCustomer

-- Churn Rate (flag from last activity)
Churn Rate :=
VAR Cutoff = TODAY() - 90
VAR ChurnedUsers =
    CALCULATETABLE (
        VALUES ( Users[UserID] ),
        Users[LastTransactionDate] < Cutoff
    )
RETURN
DIVIDE ( COUNTROWS ( ChurnedUsers ), DISTINCTCOUNT ( Users[UserID] ) )
```

</details>

<details open>
<summary><b>4) Dashboard Pages</b> — click to expand</summary>

* 💰 **Transaction Trends** — volume, value, seasonality, chip outcomes
* 🧑‍🤝‍🧑 **Customer Demographics & Behavior** — age, income, activity cohorts
* 🏷️ **Merchant Spend** — categories, top merchants, conversion
* 🔄 **Churn vs Retention** — active/inactive, cohorts, CLV, reactivation
* 🗺️ **Geography** — user distribution, high/low performing regions

</details>

---

## 🧑‍💼 Built for decision‑makers

**Design focus:** Speed to insight (CEO/Manager friendly). Minimal noise, maximum signal.
**Patterns used:** small multiples, highlight cards, drillthrough, bookmarks, tooltip pages.

<details>
<summary><b>Example KPI cards</b> (static illustration)</summary>

* Total Txn Value — **\$5.78M**
* Fraud Rate — **0.28%%**
* Chip Success — **98.38%**
* CLV — **\$8.67K**

</details>

---

## 🔎 Key insight (from the analysis)

> Most transactions came from users **50+** with income **< \$50K**.
> Highest **churn** was in the **36–50** age group.
> **Opportunity:** better targeting & engagement for **younger** and **higher‑income** segments.

---

## 🧪 Reproducibility

1. **Open** `TBank365.pbix` in Power BI Desktop (2024+).
2. (Optional) **Swap in your data** by mapping to the same columns.
3. **Refresh** to apply Power Query steps and relationships.
4. Explore **bookmarks, drillthrough,** and **tooltip** interactions.

---

## 🧱 Repo structure

```
.
├── Bank365.pbix
├── images/
│   └── data_model.png
└── README.md  ← you are here
```

---

## 🗒️ My approach (as posted on LinkedIn)

<details>
<summary><b>Click to read</b></summary>

<strong>Ever looked at raw transaction data and thought… “hmm, boring?”</strong> <strong>Not when Power BI turns it into a business story CEOs love to read!</strong>
Wrapped an end‑to‑end project for Analytix Camp PowerBI Competition.

<b>Dataset Overview</b>
→ 4 Core Tables: Transactions | Users | Cards | Merchants
→ Loaded & explored in Power Query (clean source)
→ Custom columns: Age Group, Income Level, Last Transaction Date, Active vs Inactive, Time of Day

<b>Data Modeling</b>
→ Star Schema with Transactions as Fact
→ Clean relationships for optimized reporting

<b>30+ DAX Measures</b>
→ Total Transaction Value, Fraud Rate, Chip Success, CLV, Churn & Retention

<b>Design</b>
→ Executive‑friendly visuals for quick insight without overload

<b>Coverage</b>
→ Transaction Trends, Demographics, Merchant Spend, Churn vs Retention, Geography

<b>Key Insight</b>
→ Most transactions: 50+ < \$50K income.
→ Highest churn: 36–50.
→ Opportunity: target & engage younger, higher‑income users.

</details>

---

## 🧩 Notes & assumptions

* Demo data is synthetic/anonymized.
* Measures shown are representative; adapt thresholds (e.g., churn cutoff) to your business.
* Performance tips: Use star schema, disable auto date/time, summarize columns, and cache heavy calcs in Power Query when possible.

---

## 🤝 Acknowledgments
* Power BI community for patterns on bookmarks, drillthrough, and tooltip design.

---

## 📣 Connect

<p id="-connect">
  <!-- LinkedIn -->
  <a href="https://www.linkedin.com/in/Syed-Wajdan-Zeerak" target="_blank" title="LinkedIn" style="display:inline-flex;align-items:center;padding:10px 14px;border-radius:10px;background:#0a66c2;color:#fff;text-decoration:none;margin-right:8px;">
    <!-- LinkedIn SVG -->
    <svg width="18" height="18" viewBox="0 0 24 24" fill="white" style="margin-right:8px;"><path d="M4.98 3.5C4.98 4.88 3.86 6 2.5 6S0 4.88 0 3.5 1.12 1 2.5 1s2.48 1.12 2.48 2.5zM.5 8h4V24h-4zM8.5 8h3.8v2.2h.1c.53-1 1.84-2.2 3.8-2.2 4.06 0 4.8 2.67 4.8 6.14V24h-4v-7.2c0-1.72-.03-3.94-2.4-3.94-2.4 0-2.77 1.87-2.77 3.8V24h-4z"/></svg>
    <span>Connect on LinkedIn</span>
  </a>

  <!-- Kaggle -->

  <a href="https://www.kaggle.com/zincly" target="_blank" title="Kaggle" style="display:inline-flex;align-items:center;padding:10px 14px;border-radius:10px;background:#20beff;color:#0b263b;text-decoration:none;">
    <!-- Kaggle SVG -->
    <svg width="18" height="18" viewBox="0 0 24 24" fill="#0b263b" style="margin-right:8px;"><path d="M4 3h3v7.18l6.59-6.59L15 5l-5 5 5 5-1.41 1.41L7 12.82V21H4z"/></svg>
    <span>Explore my Kaggle</span>
  </a>
</p>

<p>
  <a href="mailto:clashzeerak@example.com">youremail@example.com</a> · Issues welcome via <a href="https://github.com/YOUR-GH-USER/YOUR-REPO/issues">GitHub Issues</a>
</p>

---

## 🏷️ Tags

`#PowerBI` `#DAX` `#DataModeling` `#StarSchema` `#BusinessIntelligence` `#CustomerAnalytics` `#Churn` `#CLV` `#FraudDetection` `#DashboardDesign`

---

### ✅ How to use this README

* Replace placeholders:

  * `YOUR_PUBLISHED_LINK` → your Power BI publish‑to‑web link (if allowed)
  * `YOUR-LINKEDIN` → your LinkedIn handle
  * `YOUR-KAGGLE` → your Kaggle handle
  * `YOUR-GH-USER/YOUR-REPO` → your GitHub path
  * Email and file names as needed
* Drop screenshots/GIFs into `images/` and update paths.

---

<p align="center"><em>Power BI turns data into decisions. If you ship something with this repo, tag me — I’d love to see it!</em> 🚀</p>
