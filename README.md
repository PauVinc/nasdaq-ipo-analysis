# 📈 NASDAQ IPO Analysis — End-to-End Data Project

A complete data project analyzing 4+ years of NASDAQ IPO data — from raw scraping to an interactive Power BI dashboard.

---

## 🗺️ Project Roadmap

| Phase | Description | Status |
|-------|-------------|--------|
| Phase 1 | Data Scraping | ✅ Complete |
| Phase 2 | Data Cleaning & Preprocessing | 🔄 In Progress |
| Phase 3 | Exploratory Data Analysis (EDA) | ⏳ Upcoming |
| Phase 4 | Power BI Dashboard | ⏳ Upcoming |

---

## 📦 Dataset Overview

- **Source:** NASDAQ IPO Calendar API
- **Time Period:** January 2021 – September 2025
- **Total Records:** 2,845 IPOs
- **Format:** CSV

### Features
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
| `dealStatus` | Current status (Priced, Filed, etc.) |

---

## 🔧 Phase 1 — Data Scraping

**File:** `ipo_project_scraping.ipynb`

Two datasets were scraped and merged:
- **Priced IPOs** — 2,712 deals that successfully went public
- **Filed IPOs** — Companies that submitted filings but hadn't priced yet

Both datasets were merged via an **outer join on `dealID`**, resulting in a comprehensive 2,845-row dataset covering the full IPO lifecycle.

### Tools Used
- `requests` — API calls to NASDAQ
- `pandas` — Data manipulation and merging

---

## 🚀 Getting Started
```bash
git clone https://github.com/PauVinc/nasdaq-ipo-analysis.git
cd nasdaq-ipo-analysis
pip install requests pandas
```

Then open `ipo_project_scraping.ipynb` in Jupyter and run all cells.

---

## 📊 Phase 4 Preview — Power BI Dashboard
Coming soon — an interactive dashboard covering IPO trends, exchange comparisons, and filing-to-pricing timelines.

---

## 🤝 Connect
If you found this useful, let's connect on [LinkedIn]([https://www.linkedin.com/in/pauravi-vinchurkar-48711120a/])!
