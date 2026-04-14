# 📈 NASDAQ IPO Analysis — End-to-End Data Project

A complete data project analyzing 4+ years of NASDAQ IPO data — from raw web scraping to an interactive Power BI dashboard.

---

## 🗺️ Project Roadmap

| Phase | Description | Status |
|-------|-------------|--------|
| Phase 1 | Data Scraping | ✅ Complete |
| Phase 2 | Data Cleaning & Preprocessing | ✅ Complete |
| Phase 3 | Exploratory Data Analysis (EDA) | ✅ Complete |
| Phase 4 | Power BI Dashboard | ✅ Complete |

---

## 📦 Dataset Overview

- **Source:** NASDAQ IPO Calendar API
- **Time Period:** January 2021 – September 2025
- **Raw Records:** 2,845 IPOs
- **Cleaned Records:** 2,708 IPOs
- **Format:** CSV

---

## 🔧 Phase 1 — Data Scraping

**File:** `ipo_project_scraping.ipynb`

Two datasets were scraped and merged:
- **Priced IPOs** — 2,712 deals that successfully went public
- **Filed IPOs** — Companies that submitted filings but hadn't priced yet

Both datasets were merged via an **outer join on `dealID`**, resulting in a 2,845-row dataset covering the full IPO lifecycle.

### Raw Features
| Column | Description |
|--------|-------------|
| `dealID` | Unique identifier for each IPO deal |
| `proposedTickerSymbol` | Stock ticker symbol |
| `companyName` | Name of the company |
| `proposedExchange` | Exchange (NYSE, NASDAQ Global, NASDAQ Capital) |
| `proposedSharePrice` | IPO share price |
| `sharesOffered` | Number of shares offered |
| `dollarValueOfSharesOffered` | Total deal value in USD |
| `pricedDate` | Date the IPO was priced |
| `filedDate` | Date the IPO was filed |
| `dealStatus` | Current status (Priced / Filed) |

### Tools Used
- `requests` — API calls to NASDAQ
- `pandas` — data manipulation and merging

---

## 🧹 Phase 2 — Data Cleaning & Preprocessing

**File:** `ipo_project_datacleaning.ipynb`

### Problems Found in Raw Data

| Issue | Details |
|-------|---------|
| Duplicate columns | `companyName_x / _y`, `proposedTickerSymbol_x / _y`, `dollarValueOfSharesOffered_x / _y` from Phase 1 outer join |
| Missing values | ~950 nulls in share price, exchange, and deal status columns |
| String-formatted numbers | Dollar values stored as `$181,500,000` instead of floats |
| Inconsistent dates | Dates not in standard `datetime` format |
| Messy column names | Raw API column names not readable |

### Cleaning Steps
1. Consolidated duplicate columns using `combine_first()`
2. Stripped `$` and `,` from monetary values → converted to float in millions
3. Parsed all dates to standard `datetime` format
4. Filled missing exchange values with `"Undecided"` for filed-only IPOs
5. Renamed all columns to clean, readable names
6. Dropped irrelevant columns

### Final Cleaned Features
| Column | Description |
|--------|-------------|
| `CompanyName` | Name of the company |
| `Exchange` | NYSE, NASDAQ Global, NASDAQ Capital, Undecided |
| `FiledDate` | Date the IPO was filed |
| `PricedDate` | Date the IPO was priced |
| `DealStatus` | Priced or Filed |
| `SharesOffered(M)` | Shares offered in millions |
| `SharePrice` | IPO share price in USD |
| `TotalValue(M)` | Total deal value in millions USD |

### Tools Used
- `pandas` — merging, type conversion, null handling
- `numpy` — numeric operations

# 📈 IPO Trends Analysis — NASDAQ & NYSE

> **Exploratory Data Analysis (EDA) | Python**  
> Investigating what drives IPO fundraising across exchanges, sectors, and time (2021 onward)

---

## 📌Phase 3 - EDA

Problem Statement
> *What factors influence the total amount raised in NASDAQ and NYSE IPOs — and how do these factors differ between technology sector IPOs and others from 2021 onward?*

---

## 📁 Dataset

- **Source:** NASDAQ IPOs cleaned dataset (`nasdaq_ipos_cleaned.csv`)
- **Scope:** IPOs listed on NASDAQ and NYSE exchanges, 2021–2025
- **Key columns used:**
  - `SharePrice` — Proposed share price at IPO
  - `sharesOffered` — Number of shares offered to the public
  - `TotalValue(M)` — Total IPO offering value (in millions)
  - `Exchange` — Proposed listing exchange
  - `filedDate` / `pricedDate` — Filing and pricing dates
  - `companyName` — Used to derive a tech sector flag (`isTech`)

---

## 🔍 Analysis Structure

### 1. 📊 Univariate Analysis
- **Summary statistics** of all numeric features
- **Distribution of Total IPO Offering Value** — right-skewed with most companies raising moderate amounts; large IPOs are rare events

### 2. 🔗 Bivariate Analysis
- **Share Price vs. Total Value** — High share price alone does not guarantee large fundraising; the biggest deals are spread across price ranges
- **Total Value by Exchange** — NYSE and NASDAQ dominate high-value IPOs; NYSE Arca and NYSE MKT cater to significantly smaller offerings
- **Days from Filing to Pricing** — Most companies reach pricing within ~3 months of filing; longer waits are uncommon
- **IPO Counts per Exchange** — NASDAQ Capital Market is the most popular venue, preferred by smaller and growth-oriented companies

### 3. 📅 Time-Series Trends
- **Yearly IPO counts & median fundraising** — 2021 saw an exceptional boom; post-2021 saw fewer IPOs but the median fundraising per deal remained high, indicating market selectivity
- **Quarterly filing & pricing trends** — Surge in early 2021, followed by a sustained decline; filing and pricing dates align closely, indicating most filers complete the process within the same or next quarter

### 4. 🔄 Multivariate Analysis
- **Pairplot (colored by tech sector)** — Visualizes relationships between share price, shares offered, total value, year, and tech flag
- **Correlation heatmap** — Share price and shares offered are the strongest predictors of total value raised; filing year shows little correlation

### 5. 🏷️ Tech vs. Non-Tech Comparison
- **isTech flag** derived from company name keywords (`technology`, `tech`, `software`, `cloud`, `ai`, `semiconductor`, etc.)
- **Boxplot comparison** — No dramatic difference in core fundraising distribution between tech and non-tech IPOs; both sectors are capable of sizable offerings
- **IPO counts by year and sector** — Non-tech sectors dominate IPO volume across all years; 2021 saw a surge in both tech and non-tech listings

---

## 💡 Key Findings
| Finding | Detail |
|--------|--------|
| 📐 **Core formula** | `Money Raised = Share Price × Shares Offered` |
| 🏛️ **NYSE** | Attracts larger IPOs by funds raised |
| 📡 **NASDAQ** | Higher volume; preferred by growth-stage companies |
| 📈 **2021 anomaly** | Exceptional IPO boom; not a sustainable baseline trend |
| 🤝 **Tech ≠ bigger** | Tech and non-tech companies raised similar amounts on average |
| ⏱️ **Fast process** | Most companies price within ~3 months of filing |
| 🎯 **Market selectivity** | Post-boom, fewer but stronger companies still raised well |

---

## 🛠️ Tech Stack
| Library | Purpose |
|---------|---------|
| `pandas` | Data loading, cleaning, grouping |
| `numpy` | Numerical operations |
| `matplotlib` / `seaborn` | Visualizations (histograms, boxplots, heatmaps, pairplots) |
| `scikit-learn` | Feature engineering, encoding, classification pipeline |
| `Jupyter Notebook` | Interactive EDA workflow |

---

## 📌Phase 4 - Dashboard 

# 📈 NASDAQ IPO Analytics Dashboard — Power BI

A single-page interactive Power BI dashboard analyzing IPO activity on NASDAQ, covering deal status, exchange distribution, share pricing, and multi-year trends.

---

## 📊 Dashboard Overview

The report is built on a cleaned NASDAQ IPO dataset (`nasdaq_ipos_cleaned`) and presents the following key analytics:

### KPI Cards
| Metric | Description |
|---|---|
| **Total Companies** | Count of unique companies in the dataset |
| **Total Value Raised (M)** | Aggregate capital raised across all IPOs |
| **Total Shares Offered (M)** | Total shares offered across all deals |
| **Total Share Price** | Sum of share prices across listings |

### Visualizations

| Chart | Type | Insight |
|---|---|---|
| **IPO Count by Exchange** | Donut Chart | Distribution of IPO activity across NASDAQ exchanges |
| **Filed vs. Priced IPOs** | Pie Chart | Breakdown of deal status (Filed / Priced) |
| **Share Price & Shares Offered by Exchange** | Clustered Bar Chart | Comparative valuation metrics by exchange |
| **IPO Trend by Quarter** | Clustered Column Chart | Quarter-over-quarter IPO volume segmented by deal status |
| **Yearly IPO Trend** | Area Chart | Long-term year-wise IPO volume trend |
| **Year-wise Exchange Performance** | Treemap | Hierarchical view of exchange activity by year and quarter |

### Filters / Slicers
- **Date-wise Filter** — Filter all visuals by `FiledDate`
- **Exchange-wise Filter** — Filter all visuals by Exchange

---

## 🗂️ Data Source

- **Dataset:** `nasdaq_ipos_cleaned`
- **Key Fields:**
  - `CompanyName` — Issuing company
  - `Exchange` — NASDAQ exchange segment
  - `DealStatus` — Filed or Priced
  - `FiledDate` — IPO filing date (supports Year / Quarter / Month / Day hierarchy)
  - `SharePrice` — Offer price per share
  - `SharesOffered(M)` — Shares offered in millions
  - `TotalValue(M)` — Total deal value in millions

---

## 🛠️ Built With

- **Power BI Desktop** (2026.03 release)
- **Data Cleaning:** Pre-processed dataset loaded into the internal data model
- **Theme:** CY25SU10 base theme

---

## 🚀 Getting Started

1. Download and open `ipo_project.pbix` in [Power BI Desktop](https://powerbi.microsoft.com/desktop).
2. If prompted, refresh the data source and point it to your updated `nasdaq_ipos_cleaned` dataset.
3. Use the **Date** and **Exchange** slicers to explore specific time periods or exchange segments.
4. Publish to Power BI Service to share with stakeholders.

---

## 📁 File Structure

```
ipo_project.pbix
├── Report/Layout           # Page layout and visual configurations
├── DataModel               # Internal tabular data model
├── Report/StaticResources  # Theme files and registered images
├── Metadata                # Report metadata (version, creation info)
└── Settings                # Report-level settings
```

---

## 📌 Notes

- The dashboard contains a single report page with all visuals.
- All visuals are cross-filtered — clicking any chart element filters the rest of the page.
- The treemap and column chart both support drill-down through the date hierarchy (Year → Quarter → Month → Day).

