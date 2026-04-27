# Brazilian Tourism Data Pipeline (1989–2024)

An end-to-end data project consolidating 35 years of international tourism records from Brazil's Ministry of Tourism — from a first exploration of a single year to a scalable ETL pipeline producing analysis-ready data.

**Source:** [Ministério do Turismo — Dados Abertos](https://dados.gov.br/dados/conjuntos-dados/estimativas-de-chegadas-de-turistas-internacionais-ao-brasil) *

---

## How this project came together

It started as a way to learn Jupyter notebooks. The 1989 dataset was the first file I opened, and it turned out to be one of the most inconsistent years in the entire series, which makes sense because it was the first year of records and it was also a surprisingly good starting point.

That first exploration revealed enough quirks (schema variations, missing values, encoding issues) that going straight into automation without understanding the data first would have been a mistake. So the project naturally split into two phases.

---

## Phase 1 — Exploration (1989)

`notebooks/Exploration_1989.ipynb`

A manual, single-year analysis focused on understanding the data before building anything. This notebook covers:

- First contact with the raw structure: 17,000 rows, 12 columns, Latin-1 encoding
- Identifying 588 null values in `Chegadas` (arrivals), isolated to Fluvial access in Mato Grosso do Sul
- Basic cleaning: dropping auxiliary columns, handling nulls, trimming whitespace
- Visualizations: top 10 countries by arrivals, transport mode breakdown, state gateways, and monthly seasonality

Some findings from 1989 worth noting: Argentina dominated arrivals by a wide margin, most border-country tourists came by land, the South region was the main gateway, and January/February marked the clear peak season — patterns that will be interesting to compare against the full 35-year picture.

---

## Phase 2 — Scalable Pipeline (1989–2024)

`notebooks/tourism_1989_2024.ipynb`

With the data understood, this notebook builds the automated ETL to process all 36 files at once. The main challenges:

**Schema drift:** Column names changed across decades — `Ano`, `ano`, abbreviated versions. A dynamic mapping layer normalizes every file to the same canonical schema on ingestion.

**Missing data patterns:** Three distinct patterns were identified and handled differently depending on the likely cause — localized method gaps, state-wide reporting blackouts, and systemic collection shifts. All imputed records are tracked via a `data_quality_flag` column so downstream analysis can filter them when precision matters.

**Semantic inconsistencies:** Variations like `aérea`/`aéreo` and `marco`/`março` were caught through a categorical audit and corrected via a normalization dictionary.

The pipeline produces a single consolidated dataset of ~953,000 rows across 8 columns, with full data lineage preserved.

**Note on visualizations:** this notebook intentionally has no charts. The analysis layer moves to Power BI, where the full 35-year dataset will be explored properly.

---

## Output Layers

| Format | Purpose |
|--------|---------|
| Parquet (partitioned by year) | Big Data / analytics engines |
| SQLite (`fact_tourism_arrivals`) | Relational queries, Data Warehouse simulation |
| CSV compressed (.gz) | Sharing / legacy systems |
| CSV plain | Direct BI tool integration (Power BI) |

---

## Project Structure

```
├── data/
│   ├── raw/               # Original CSVs from Ministério do Turismo (not versioned)
│   └── processed/         # Pipeline outputs (generated, not versioned)
├── notebooks/
│   ├── Exploration_1989.ipynb
│   └── tourism_1989_2024.ipynb
└── README.md
```

---

## How to Run

**Requirements:** Python 3.12, Jupyter

```bash
# Phase 1 (Exploration)
pip install pandas numpy seaborn matplotlib

# Phase 2 (Pipeline)
pip install pandas numpy pyarrow fastparquet
```

Download the raw CSVs from the [source](https://dados.gov.br/dados/conjuntos-dados/estimativas-de-chegadas-de-turistas-internacionais-ao-brasil) *, place them in `data/raw/`, and run the notebooks in order.

---

## What's next

The ETL is complete. Phase 3 will take this data into Power BI to extract insights from the full historical series — pandemic impact, long-term growth trends, shifts in country of origin, and whether the patterns from 1989 held across 35 years.

---

**Fabio Iamashita** — [LinkedIn](https://linkedin.com/in/fabio-iamashita)


\* Note: If the source link returns a 403 error, please refresh the page (F5)
