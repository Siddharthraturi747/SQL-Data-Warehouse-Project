# 🏛️ SQL Server Data Warehouse Project

An end-to-end data warehouse built in **SQL Server** using the **Medallion Architecture** (Bronze → Silver → Gold). The project ingests raw CRM and ERP data from CSV files, cleans and standardizes it, and exposes a business-ready **star schema** for analytics, BI reporting, and machine learning.

---


| Layer | Purpose | Object Type | Load Strategy | Transformations | Data Model |
|-------|---------|-------------|---------------|-----------------|------------|
| **Bronze** | Raw data, ingested as-is | Tables | Batch processing, full load, truncate & insert | None | None (as-is) |
| **Silver** | Cleaned, standardized data | Tables | Batch processing, full load, truncate & insert | Cleansing, standardization, normalization, derived columns, enrichment | None (as-is) |
| **Gold** | Business-ready data | Views | No load (views over Silver) | Data integration, aggregations, business logic | Star schema, flat tables, aggregated tables |

**Sources:** CRM and ERP systems delivered as CSV files in folders.
**Consumers:** BI & reporting, ad-hoc SQL queries, machine learning.

---

## 🧰 Tech Stack

- **Database:** Microsoft SQL Server
- **Language:** T-SQL
- **ETL:** Stored procedures (batch, full load)
- **Tools:** SQL Server Management Studio (SSMS), Git, GitHub
- **Modeling:** Star schema (fact and dimension views)

---

## 📁 Repository Structure

```
sql-data-warehouse-project/
├── datasets/              # Source CSV files (CRM and ERP)
├── docs/                  # Architecture diagram, data model, naming conventions
├── scripts/
│   ├── init_database.sql  # Creates the database and schemas (bronze, silver, gold)
│   ├── bronze/            # DDL and load procedure for raw data
│   ├── silver/            # DDL and cleaning/transformation procedure
│   └── gold/              # Views forming the star schema
├── tests/                 # Data quality checks
├── README.md
└── .gitignore
```

---

## 🔄 How It Works

### 🥉 Bronze Layer: Raw Ingestion
- Loads CSV files from CRM and ERP into SQL Server tables **without any transformation**
- Uses a stored procedure with **truncate & insert** for repeatable full loads
- Preserves the source data exactly for traceability

### 🥈 Silver Layer: Cleansing and Standardization
- **Data cleansing:** removes duplicates, trims whitespace, handles nulls and invalid values
- **Standardization:** consistent formats for dates, codes, and categorical values
- **Normalization and enrichment:** derived columns and added business context
- Loaded from Bronze through a stored procedure (truncate & insert)

### 🥇 Gold Layer: Business-Ready Model
- Built as **views** on top of Silver, so there is no separate load step
- Integrates CRM and ERP data into a single model
- **Star schema** with dimension and fact views for easy analytics

---

## 🚀 Getting Started

### Prerequisites
- SQL Server (Express or Developer edition works)
- SQL Server Management Studio (SSMS) or Azure Data Studio

### Run Order
1. Clone the repo:
   ```bash
   git clone https://github.com/<your-username>/sql-data-warehouse-project.git
   ```
2. Update the CSV file paths in the Bronze load procedure to match your local machine.
3. Run the scripts in this order:
   1. `scripts/init_database.sql`
   2. `scripts/bronze/` (DDL, then the load procedure)
   3. `scripts/silver/` (DDL, then the load procedure)
   4. `scripts/gold/` (create views)
4. Execute the load procedures:
   ```sql
   EXEC bronze.load_bronze;
   EXEC silver.load_silver;
   ```
5. Query the Gold layer:
   ```sql
   SELECT TOP 10 * FROM gold.fact_sales;
   ```

> Procedure and object names above are examples. Adjust them to match your scripts.

---

## ✅ Data Quality Checks

Quality checks live in the `tests/` folder and cover:
- Duplicate and null primary keys
- Unwanted spaces in text fields
- Invalid dates and date ordering
- Consistency between related columns (for example sales = quantity × price)
- Referential integrity between fact and dimension views

---

## 💡 Key Learnings

- Separating layers makes pipelines easier to debug, test, and maintain
- Stored procedures make ETL repeatable and easy to schedule
- Most of the real effort goes into understanding and cleaning messy source data
- Good data modeling matters as much as good SQL

---

## 🔮 Future Improvements

- Orchestrate the pipeline with **Apache Airflow**
- Rebuild the transformations with **dbt**
- Add incremental loads in place of full loads
- Add logging and error handling to the load procedures

---

## 📄 License

This project is licensed under the MIT License. Feel free to use and adapt it with attribution.
