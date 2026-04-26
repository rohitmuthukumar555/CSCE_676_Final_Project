# Data Mining Course Project: Dataset Selection & Exploratory Data Analysis

**Course:** CSCE 676 (or equivalent Data Mining)  
**Deliverable:** Dataset identification, comparative analysis, selection, and EDA.

---

## (A) Identification of Candidate Datasets

### Candidate 1: UCI Online Retail Dataset

| Attribute | Description |
|-----------|-------------|
| **Dataset name and source** | **Online Retail Dataset** — UCI Machine Learning Repository ([archive.ics.uci.edu/dataset/352/online-retail](https://archive.ics.uci.edu/dataset/352/online-retail)); also on Kaggle. |
| **Course topic alignment** | **Frequent itemsets and association rule mining** (market basket analysis). |
| **Potential beyond-course techniques** | **Sequential pattern mining** (e.g., PrefixSpan, SPADE) using `InvoiceDate` and `CustomerID`; **Bonferroni-corrected correlations** or **Chi-squared tests** for item associations. |
| **Dataset size and structure** | ~541,909 transaction lines; variable-length baskets per invoice; multiple rows per transaction (one per line item). |
| **Data types** | `InvoiceNo`, `StockCode`, `Description`, `Quantity`, `InvoiceDate`, `UnitPrice`, `CustomerID`, `Country`. |
| **Target variable(s)** | None for unsupervised pattern mining; optional: `Quantity` or revenue for supervised tasks. |
| **Licensing or usage constraints** | **CC BY 4.0** (UCI). Free for research and education with attribution. |

---

### Candidate 2: Cora Citation Network Dataset

| Attribute | Description |
|-----------|-------------|
| **Dataset name and source** | **Cora** — Citation network; LINQS (UCSC), e.g. [linqs-data.soe.ucsc.edu/public/lbc/cora.tgz](https://linqs-data.soe.ucsc.edu/public/lbc/cora.tgz); also on Papers With Code and GitHub. |
| **Course topic alignment** | **Graph mining**: centrality, **PageRank**, **community detection** (e.g., Louvain, label propagation). |
| **Potential beyond-course techniques** | **Graph neural networks (GNN)** for node classification; **node embeddings** (e.g., Node2Vec, GraphSAGE). |
| **Dataset size and structure** | 2,708 nodes (papers), 5,429 directed edges (citations); node feature matrix 2,708 × 1,433 (binary bag-of-words). |
| **Data types** | `.content`: paper ID, 1,433 binary word features, class label; `.cites`: citation pairs (paper1, paper2). |
| **Target variable(s)** | Class label (one of 7 research areas: e.g., Neural_Networks, Theory). |
| **Licensing or usage constraints** | Research use; commonly used in publications. Check LINQS/author terms; no restrictive commercial barrier for academic use. |

---

### Candidate 3: Amazon Product Reviews (e.g., Electronics or subset)

| Attribute | Description |
|-----------|-------------|
| **Dataset name and source** | **Amazon Product Reviews** — e.g. **Amazon Reviews 2018** ([nijianmo.github.io/amazon/](https://nijianmo.github.io/amazon/)) or **Amazon Reviews'23** (McAuley Lab / Hugging Face); per-category CSV/JSON. |
| **Course topic alignment** | **Text mining**: TF-IDF, **embeddings** (e.g., word2vec, doc2vec), sentiment or classification. |
| **Potential beyond-course techniques** | **Topic modeling** (LDA, NMF); **transformer-based embeddings** (e.g., sentence-BERT) for semantic similarity. |
| **Dataset size and structure** | Millions of reviews; subset (e.g., Electronics) ~1–5M rows; each row: `reviewerID`, `asin`, `rating`, `reviewText`, `summary`, `unixReviewTime`, etc. |
| **Data types** | Categorical IDs, numeric ratings, **text** (review body/summary), timestamps. |
| **Target variable(s)** | Optional: `rating` (1–5), helpfulness votes, or derived sentiment. |
| **Licensing or usage constraints** | Research datasets; follow McAuley Lab / provider terms. Often used in academic papers with citation. |

---

## (B) Comparative Analysis of Datasets

| Dimension | **Online Retail** | **Cora** | **Amazon Reviews** |
|-----------|-------------------|----------|---------------------|
| **Supported data mining tasks** | **Course:** Frequent itemsets, association rules. **External:** Sequential pattern mining, correlation/Chi-squared. | **Course:** Graph centrality, PageRank, community detection. **External:** GNN node classification, node embeddings. | **Course:** Text mining, TF-IDF, embeddings. **External:** Topic modeling (LDA), transformer embeddings. |
| **Data quality issues** | Missing `CustomerID` (~25%); cancelled invoices (prefix 'C'); negative quantities (returns); duplicate/multiline transactions. | Small, clean benchmark; possible isolated nodes; directed edges only. | Noisy text (typos, slang); missing `reviewText`; rating imbalance; long-tail products. |
| **Algorithmic feasibility** | Apriori/FP-Growth feasible on basket table (aggregate by invoice); sequential mining feasible per customer with reasonable support. | Graph small enough for in-memory (NetworkX, PyG); PageRank and community detection trivial; GNN training feasible on CPU/GPU. | LDA/topic modeling and embeddings feasible on subset (e.g., 50k–500k reviews); full 2018 dataset may need sampling or streaming. |
| **Bias considerations** | **Recommendation/popularity bias:** best-sellers dominate; **geographic bias:** UK-focused; **temporal:** single year. | **Citation bias:** certain fields cite more; **label bias:** class distribution uneven. | **Selection bias:** voluntary reviews; **demographic bias:** reviewer population not representative; **product bias:** popular items over-represented. |
| **Ethical considerations** | Low risk; anonymized transactions; no PII. Use for learning, not targeting individuals. | Low risk; public academic metadata. No harm from citation analysis. | Review text could reflect personal experience; avoid re-identification; use for aggregate analysis only. |

---

## (C) Dataset Selection

**Selected dataset: UCI Online Retail Dataset**

**Reasons:**

1. **Directly supports course content:** Frequent itemsets and association rules are a core topic; the dataset is naturally formatted as transaction baskets.
2. **Clear beyond-course angle:** Sequential pattern mining (order of purchases over time) is not typically covered in class and allows meaningful comparison with unordered itemset rules.
3. **Interpretable and portfolio-friendly:** Results (e.g., “customers who buy X often buy Y” or “sequence A → B → C”) are easy to explain in interviews and reports.
4. **Manageable size and tooling:** ~540K rows can be handled in pandas; Apriori/FP-Growth (e.g., `mlxtend`) and sequential algorithms (e.g., `prefixspan`) are readily available in Python.
5. **EDA richness:** Basket size distribution, item frequency, sparsity, temporal gaps, and missingness all motivate both course and external techniques.

**Trade-offs:**

- **No native text component** — no text mining on this dataset unless product descriptions are used separately.
- **Limited supervised labels** — no explicit target unless we create one (e.g., predict next purchase or segment).
- **Data quality work required** — handling cancellations, missing `CustomerID`, and returns is part of the EDA and cleaning narrative.

---

## (D) Exploratory Data Analysis (Selected Dataset: Online Retail)

*(Summary; full EDA is in the Jupyter notebook `notebooks/01_eda_online_retail.ipynb`.)*

Planned EDA steps:

1. **Data basics:** Load CSV/Excel; shape; dtypes; missing values (especially `CustomerID`); duplicate rows.
2. **Transaction construction:** Group by `InvoiceNo` to form baskets; distribution of **basket sizes** (items per transaction).
3. **Item frequency:** Count of transactions per `StockCode`/`Description`; **top items**; frequency distribution (e.g., how many items appear in &lt;1% of transactions).
4. **Sparsity:** Number of unique items vs. transactions; item co-occurrence sparsity (e.g., pairwise matrix density).
5. **Temporal:** `InvoiceDate` range; transactions per day/month; **gaps between consecutive transactions** per customer (for sequential mining).
6. **Data cleaning and bias:** Identify cancelled invoices (`InvoiceNo` starting with 'C'), negative `Quantity`; assess **geographic and time coverage** for bias.
7. **Initial observations for techniques:** e.g., “Most items appear in few transactions” → need for support thresholds and possibly sequential patterns to capture temporal structure.

---

## (E) Initial Insights and Direction

**Observation:** Many items appear in fewer than 1% of transactions (long tail). High minimum support may miss interpretable rules; very low support leads to noisy rules.

**Hypothesis:** Sequential patterns (e.g., “bought A then B within 30 days”) may reveal structure that unordered frequent itemsets miss, especially for repeat customers.

**Potential research questions:**

- How do different **minimum support** and **confidence** thresholds affect the number and quality of association rules?
- Do **sequential patterns** (with time windows) reveal purchase behavior that **frequent itemsets** do not?
- How does **filtering** (e.g., drop cancellations, require `CustomerID`) change basket size and item frequency distributions?

---

## (F) GitHub Portfolio

- **Repository:** [Add your public GitHub repo URL here]
- **First notebook:** `notebooks/01_eda_online_retail.ipynb` (exploratory data analysis for Online Retail).
- **README:** See project root `README.md` for overview, setup, and dataset instructions.

---

*End of report.*
