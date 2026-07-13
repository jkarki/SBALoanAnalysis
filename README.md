# SBA Loan Default Analysis

An exploration of what predicts default in SBA-guaranteed small business loans, covering loans approved between 1990 and 2014.

## Live Dashboard

[View interactive Tableau dashboard](https://public.tableau.com/app/profile/utkarsh.karkii/viz/SBALoanAnalysis_17839556807550/Dashboard1)

## The Question

The SBA guarantees a portion of small business loans to encourage banks to lend to businesses that might otherwise be too risky. But not all loans are created equal. Does loan size predict default? Does a higher SBA guarantee percentage make a loan safer, or does it just change who's willing to lend? Are new businesses riskier bets than existing ones?

## Findings

**Smaller loans default far more often:** Loans under $50K defaulted at a 28.8% rate, compared to 20.7% for $50K-$150K, 11.3% for $150K-$500K, and just 9.0% for loans over $500K. Loan size and default risk move in opposite directions across the board.

**Guarantee percentage doesn't behave the way you'd expect:** Loans with a 90%+ SBA guarantee defaulted at only 1.9%, the lowest of any tier, while loans guaranteed at 50-75% defaulted at 26.6%, the highest of any tier. The relationship isn't a straight line — it likely reflects which loan programs and borrower types fall into each guarantee bracket rather than the guarantee itself driving behavior.

**New businesses are modestly riskier than existing ones:** New businesses defaulted at 21.3% versus 19.9% for existing businesses — a real but smaller gap than loan size or guarantee tier produce.

**Overall base rate:** Across all 687,942 loans in the dataset, 139,674 defaulted — a 20.3% overall default rate.

## Data

- **Source:** SBA national loan-level dataset (~899K records)
- **Scope:** Loans approved 1990-2014, spanning 51 states/territories and 24 NAICS industry sectors
- **Granularity:** One row per loan in the raw data; SQL output aggregated by state, industry sector, approval year, business type, loan size bucket, and guarantee tier

## Pipeline

**Data ingestion (Python/pandas)**
- Loaded the raw SBA national loan dataset (~899K rows, 27 columns)
- Checked column types and null counts across the full dataset

**Feature engineering (Python/pandas)**
- Dropped records missing loan status or business-existence flags
- Created a binary default flag from loan status (charged off vs. paid in full)
- Cleaned dollar-formatted columns and computed the SBA guarantee percentage (SBA-approved amount / gross approved amount)
- Derived a two-digit NAICS sector code from the full NAICS code
- Selected and renamed analytical columns, output to clean CSV

**Database loading and analysis (PostgreSQL via DBeaver)**
- Loaded the cleaned dataset into PostgreSQL
- Wrote aggregate queries by state, industry, year, business type, loan size bucket, and guarantee tier to surface default patterns
- Exported query results to CSV

**Visualization (Tableau Public)**
- Built an interactive dashboard with four connected views: Industry, Loan Size, Over Time, and State Map

## Tools

- **Python:** pandas, NumPy
- **Database:** PostgreSQL, DBeaver
- **Visualization:** Tableau Public Desktop
- **Version control:** Git, GitHub

## Repository Structure

```
SBALoanAnalysis/
├── data/
│   ├── raw/
│   │   └── SBAnational.csv           # Source data (not tracked, see .gitignore)
│   └── clean/
│       └── final_sba_data.csv        # Final cleaned dataset
├── notebooks/
│   └── SBALoanAnalysis.ipynb         # pandas data cleaning and feature engineering
├── sql/
│   └── sba_analysis.csv              # PostgreSQL query result export
└── Tableau/
    └── SBALoanAnalysis.twb           # Tableau workbook
```

## What I'd Do Differently

- Bring the data forward past 2014 to see if default patterns have shifted
- Control for macroeconomic conditions (loans approved right before 2008 vs. during recovery look very different)
- Dig into why the guarantee-tier relationship isn't monotonic instead of just flagging it
- Build a predictive model on top of the descriptive analysis

## Author

Jay Karki | [LinkedIn](https://www.linkedin.com/in/jaykarki/) | jayukarki11@gmail.com
