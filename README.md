# CSCE 676 Data Mining Project: Online Retail Analysis

## Project Overview

This project analyzes the UCI Online Retail dataset to study transaction behavior and extract actionable patterns. The analysis focuses on frequent itemsets and association rules from the course, plus sequence-oriented pattern insights beyond the core syllabus.

**Selected dataset:** [UCI Online Retail Dataset](https://archive.ics.uci.edu/dataset/352/online-retail)

👉 **Main deliverable notebook:** `main_notebook.ipynb`  
🎥 **Project video:** [Watch here](https://www.youtube.com/shorts/xJzItIV3ZSE)

## Research Questions

1. Which products are most frequently purchased together?
2. What association rules are strongest by support, confidence, and lift?
3. Are there meaningful temporal or repeat-purchase patterns that suggest sequential behavior?
4. What business recommendations can be derived from these patterns?

## Data

- **Source:** UCI Machine Learning Repository - Online Retail
- **Size:** ~541,909 transaction line items
- **Columns used:** `InvoiceNo`, `StockCode`, `Description`, `Quantity`, `InvoiceDate`, `UnitPrice`, `CustomerID`, `Country`
- **License:** CC BY 4.0

### How to Get Data

1. Download from [UCI](https://archive.ics.uci.edu/dataset/352/online-retail) or [Kaggle mirror](https://www.kaggle.com/datasets/viridianachow/online-retail-uci-dataset).
2. Save as `data/Online Retail.csv`.

### Preprocessing Summary

- Remove cancellations/returns and invalid quantity-price rows.
- Normalize invoice timestamps and transaction grouping.
- Build basket and customer-level structures for mining.

## Reproducibility

This project was developed in Google Colab.

1. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
2. Place dataset at `data/Online Retail.csv`.
3. Run notebooks/scripts in this order:
   - `checkpoints/checkpoint_1.ipynb`
   - `checkpoints/checkpoint_2.ipynb`
   - `main_notebook.ipynb`

### Python and Key Dependencies

- `python`: `3.12.13`
- `pandas`: `>=1.5.0`
- `numpy`: `>=1.21.0`
- `matplotlib`: `>=3.5.0`
- `seaborn`: `>=0.12.0`

Full dependency list: `requirements.txt`.

## Repository Structure

```text
CSCE676-datamining-project/
├── README.md
├── REPORT.md
├── requirements.txt
├── FINAL_SUBMISSION_CHECKLIST.md
├── data/
│   └── .gitkeep
├── checkpoints/
│   ├── checkpoint_1.ipynb
│   └── checkpoint_2.ipynb
├── main_notebook.ipynb
└── notebooks/
    └── 01_eda_online_retail.ipynb
```

## Results Summary

Initial analysis indicates strong co-purchase behavior among recurring inventory groups and seasonally active product bundles. The final notebook reports top rules and interprets their practical implications for bundle design and promotion strategy.

## Notes

- `REPORT.md` contains earlier phase artifacts and supporting write-up.
- Before submission, confirm the repository is public using an incognito browser session.
