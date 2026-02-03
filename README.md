# 🚀 Brazil Tourism Data Pipeline (1989 - 2024)

This project builds an end-to-end data pipeline to analyze international tourism in Brazil over the last 35 years. It demonstrates the transition from a manual exploratory analysis to a scalable, automated ETL process.

## 🛠️ Tech Stack
* **Language:** Python 3.12
* **Libraries:** Pandas, Numpy, Seaborn, Matplotlib
* **Architecture:** Modular ETL (Extract, Transform, Load)

## 📈 Project Roadmap

### Phase 1: Deep Dive & Legacy Discovery (Completed)
Before automating, I performed a standalone analysis of the 1989 dataset to understand the "root" of the data structure.
* **Notebook:** `Exploration_1989.ipynb`
* **Focus:** Data cleansing patterns, historical bias identification, and baseline visualization of the late 80s tourism scenario.

### Phase 2: Scalable Pipeline Construction (In Progress)
Current focus is on consolidating the full historical series (1989-2024).
* **Notebook:** `tourism_1989_2024.ipynb`
* **Status:** Successfully implemented automated schema mapping and aggregation for ~950k records.

## 💡 Technical Challenges & Solutions

### 1. Handling Schema Drift
Over three decades, CSV files changed column names and structures multiple times.
* **Solution:** Developed a dynamic **Schema Mapping** strategy to normalize inconsistent headers into a canonical format during ingestion.

### 2. Data Integrity & Missing Values
Legacy datasets frequently contain inconsistent formatting, empty strings, or missing values in count fields.
* **Solution:** Implemented an automated **Quality Health Report**. The pipeline performs safe numeric casting and audits the "null rate" per file. This ensures the final dataset remains consistent for analysis while providing a clear diagnostic of data gaps from historical records.

### 3. Historical Consistency
Deciding how to handle missing data from different eras (e.g., periods with lower data maturity).
* **Solution:** Chose to preserve `NaN` values for corrupted records instead of defaulting to zero, avoiding statistical bias in future temporal analysis.

## 📁 Project Structure
- `notebooks/`: Development notebooks (Exploration vs. Pipeline).
- `data/raw/`: Source CSV files (not included due to size).

## 🧪 How to Run
1. Clone this repository.
2. Ensure `data/raw` is populated with official CSVs from [Ministério do Turismo](https://dados.gov.br/dados/conjuntos-dados/estimativas-de-chegadas-de-turistas-internacionais-ao-brasil).
3. Execute tourism_1989_2024.ipynb to run the ETL pipeline and generate the consolidated dataset.

---
**Author:** [Fabio Iamashita] | [[LinkedIn](https://linkedin.com/in/fabio-iamashita)]