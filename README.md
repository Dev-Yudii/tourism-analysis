# Brazilian Tourism Data Pipeline (1989–2024)

An end-to-end data project consolidating 35 years of international tourism records from Brazil's Ministry of Tourism — from a first exploration of a single year to a reproducible ETL pipeline and an interactive Power BI analysis.

## How this project came together

This project started as a simple exploration of a single tourism dataset and gradually evolved as I wanted to understand not only the data itself, but also how to build a process capable of handling the complete historical series.

The project is divided into three phases:

* **Phase 1 — Exploration:** understanding the structure and characteristics of the 1989 dataset.
* **Phase 2 — ETL Pipeline:** processing and consolidating the historical data from 1989 to 2024.
* **Phase 3 — Power BI Analysis:** using the consolidated dataset to explore long-term tourism trends and patterns.

---

## Phase 1 — Exploration (1989)

The project began with the exploration of the 1989 dataset in `notebooks/Exploration_1989.ipynb`.

The original file contained approximately 17,000 records across 12 columns. During the initial exploration, I identified characteristics that would later influence the data processing pipeline:

* The dataset used **Latin-1 encoding**.
* The `Chegadas` column contained **588 missing values**.
* Different variables represented aspects such as country of origin, Brazilian state, point of entry, transportation mode and month.
* Argentina was the main country of origin in the dataset.
* Land transportation was particularly relevant for neighboring countries.
* The South region played an important role as a gateway for international arrivals.
* Arrivals showed noticeable seasonality, especially around January and February.

This first analysis was useful not only for generating insights, but also for understanding the inconsistencies and variations that would need to be handled when working with the complete historical series.

---

## Phase 2 — ETL Pipeline (1989–2024)

After exploring the 1989 data, the project was expanded to process the complete historical series in `notebooks/tourism_1989_2024.ipynb`.

The pipeline processes **36 annual files**, dealing with differences that appeared between years instead of assuming that every file followed exactly the same structure.

The main steps include:

* Reading the original CSV files with the appropriate encoding.
* Standardizing column names and data types.
* Handling schema changes between different years.
* Identifying and dealing with missing data.
* Addressing semantic inconsistencies across the historical files.
* Adding year information to preserve data lineage.
* Consolidating the datasets into a single analysis-ready table.
* Generating different output formats for different use cases.

The resulting dataset contains approximately **953,000 records across 8 columns**, covering the period from 1989 to 2024.

The ETL notebook intentionally contains no charts. Once the historical data was consolidated and cleaned, the project moved into Power BI for the analysis and visualization stage.

### Output layers

The pipeline generates several outputs from the consolidated data:

* **Parquet:** partitioned by year for efficient analytical processing.
* **SQLite:** stored as `fact_tourism_arrivals`, providing a structured database version of the consolidated dataset.
* **CSV:** compressed version of the processed dataset.
* **CSV for Power BI:** a plain CSV version used as the source for the dashboard.

The SQLite database is used as a catalog and structured representation of the processed data, while the Parquet and CSV outputs provide alternatives depending on the analysis workflow.

---

## Phase 3 — Power BI Analysis

With the historical data consolidated, the project moved from data preparation to analysis in Power BI.

The Power BI report is available in:

`powerbi/`

The analysis uses the complete 1989–2024 series to explore how international tourism to Brazil changed over time.

The dashboard focuses on questions such as:

* How did international tourist arrivals evolve over the 35-year period?
* Which countries and regions were the main sources of visitors?
* How did the importance of different points of entry change?
* What role did each transportation mode play over time?
* Which seasonal patterns can be observed in the historical data?
* How did the COVID-19 pandemic affect international tourism?
* Did patterns observed in the initial 1989 exploration remain consistent over the following decades?

The Power BI report provides an interactive way to explore these patterns rather than limiting the analysis to static charts generated during the data preparation stage.

A PDF export of the report is also included in the `powerbi/` directory for quick access to the analysis without requiring Power BI Desktop.

---

## Project structure

```text
├── data/
│   ├── raw/
│   └── processed/
├── notebooks/
│   ├── Exploration_1989.ipynb
│   └── tourism_1989_2024.ipynb
├── powerbi/
│   ├── tourism-analysis.pbix
│   └── tourism-analysis.pdf
├── .gitignore
├── .python-version
├── pyproject.toml
├── README.md
└── uv.lock
```

The raw datasets are not included in the repository. They can be downloaded from the original source and placed in `data/raw/`.

---

## Data source

The data comes from the Brazilian Ministry of Tourism's open data portal:

**Ministério do Turismo — Dados Abertos**

The original datasets cover international tourism arrivals in Brazil across different years and contain information related to origin, destination, entry point, transportation mode and time period.

---

## How to run

### Requirements

* Python 3.12
* [uv](https://docs.astral.sh/uv/)

### 1. Clone the repository

```bash
git clone https://github.com/Dev-Yudii/tourism-analysis.git
cd tourism-analysis
```

### 2. Install the environment

```bash
uv sync
```

### 3. Download the raw datasets

Download the annual CSV files from the Ministry of Tourism's open data portal and place them in:

```text
data/raw/
```

The files should contain the datasets for the period from 1989 to 2024.

### 4. Run the notebooks

The notebooks should be followed in order:

```text
notebooks/Exploration_1989.ipynb
        ↓
notebooks/tourism_1989_2024.ipynb
        ↓
powerbi/
```

The first notebook explores the original 1989 dataset.

The second processes the complete historical series and generates the analysis-ready outputs.

The resulting dataset can then be loaded into Power BI to explore the historical trends through the report in `powerbi/`.

---

## Project status

The project is currently **complete**, covering the full workflow from initial data exploration to historical data processing and BI analysis:

**Exploration → ETL → Analysis**

This version represents the current scope of the project and is considered finished for now. I may come back to it in the future if I want to experiment with a new tool, technique or analytical approach.
