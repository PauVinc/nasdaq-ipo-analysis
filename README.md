# 📈 NASDAQ IPO Analysis — End-to-End Data Project

A complete data project analyzing 4+ years of NASDAQ IPO data — from raw web scraping to an interactive Power BI dashboard.

---

## 🗺️ Project Roadmap

| Phase | Description | Status |
|-------|-------------|--------|
| Phase 1 | Data Scraping | ✅ Complete |
| Phase 2 | Data Cleaning & Preprocessing | ✅ Complete |
| Phase 3 | Exploratory Data Analysis (EDA) | 🔄 In Progress |
| Phase 4 | Power BI Dashboard | ⏳ Upcoming |

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
