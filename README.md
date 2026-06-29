# ShopFlow Pipeline 🛒

An end-to-end data engineering pipeline built on Databricks — ingesting raw e-commerce data, transforming it through a Bronze → Silver → Gold medallion architecture, and surfacing insights via a Tableau Public dashboard.

---

## 📊 Live Dashboard

👉 **[[View ShopFlow Sales Dashboard on Tableau Public](https://public.tableau.com/your-link-here)](https://public.tableau.com/app/profile/charvak.chavare/viz/shopflowsalesdasbord/Dashboard1)**



---

## 🏗 Architecture

```
CSV Data (Python + Faker)
        ↓
  Databricks DBFS / Volume
        ↓
  Bronze Layer  →  Raw Delta tables (no transforms)
        ↓
  Silver Layer  →  Cleaned, typed, deduplicated
        ↓
  Gold Layer    →  Aggregated, business-ready tables
        ↓
  Tableau Public Dashboard
```

---

## 🛠 Tech Stack

| Layer | Tool |
|---|---|
| Data Generation | Python, Faker, pandas |
| Storage | Databricks (Free Tier), Delta Lake |
| Transformation | PySpark, Spark SQL |
| Orchestration | Databricks Notebooks |
| Visualisation | Tableau Public |
| Version Control | Git, GitHub |

---

## 📁 Project Structure

```
shopflow-pipeline/
├── README.md
├── assets/
│   └── dashboard_preview.png
├── data_generation/
│   └── generate_data.py          # Generates customers, products, orders CSVs
├── databricks/
│   ├── 01_bronze_load.py         # Raw CSV → Bronze Delta tables
│   ├── 02_silver_transform.py    # Bronze → Silver (clean, cast, dedupe)
│   ├── 03_gold_aggregations.sql  # Silver → Gold (aggregated business tables)
│   └── 04_export_gold.py        # Export Gold tables to CSV for Tableau
└── tableau/
    └── shopflow_dashboard.twbx   # Tableau Public workbook
```

---

## 🥉🥈🥇 Medallion Architecture

### Bronze — Raw Layer
Stores data exactly as received from the source. No transformations applied. Serves as the single source of truth for reprocessing.

**Tables:** `shopflow.bronze.customers`, `shopflow.bronze.products`, `shopflow.bronze.orders`

### Silver — Cleaned Layer
Applies type casting, deduplication, null handling, and string normalisation. Adds derived columns like `revenue` and `order_month`.

**Tables:** `shopflow.silver.customers`, `shopflow.silver.products`, `shopflow.silver.orders`

### Gold — Aggregated Layer
Business-ready aggregations designed for direct consumption by BI tools and analysts.

| Table | Description |
|---|---|
| `gold.daily_revenue` | Revenue, orders, and unique customers per day |
| `gold.monthly_revenue` | Month-over-month revenue with growth % |
| `gold.top_products` | Best sellers ranked by revenue |
| `gold.category_performance` | Revenue share by product category |
| `gold.customer_segments` | High / Mid / Low value customer tiers |

---

## 🚀 How to Run

### Prerequisites
- Databricks Free Tier account ([sign up here](https://community.cloud.databricks.com))
- Python 3.8+
- Tableau Public ([download here](https://public.tableau.com/en-us/s/download))

### Step 1 — Generate data
```bash
pip install faker pandas
python data_generation/generate_data.py
# Output: customers.csv, products.csv, orders.csv
```

### Step 2 — Upload to Databricks
- Go to **Catalog → Add data → Upload files to volume**
- Upload all 3 CSVs to `/Volumes/shopflow/raw/csvfiles/`

### Step 3 — Run notebooks in order
Open each notebook in Databricks and run all cells:
```
01_bronze_load.py       → creates shopflow.bronze.*
02_silver_transform.py  → creates shopflow.silver.*
03_gold_aggregations.sql → creates shopflow.gold.*
04_export_gold.py       → exports CSVs for Tableau
```

### Step 4 — Open in Tableau Public
- Connect to each Gold CSV as a Text File data source
- Or open `tableau/shopflow_dashboard.twbx` directly

---

## 📈 Key Metrics (Sample Output)

| Metric | Value |
|---|---|
| Total Orders | 5,000 |
| Completed Orders | ~3,000 |
| Total Revenue | ~$142,000 |
| Unique Customers | 500 |
| Products | 50 across 7 categories |
| Date Range | 12 months |

---

## 💡 What I Learned

- Implementing the **medallion architecture** (Bronze/Silver/Gold) in Databricks
- Writing production-quality **PySpark transformations** with explicit type casting, deduplication, and null handling
- Using **Delta Lake** features: ACID transactions, schema enforcement, and `DESCRIBE HISTORY`
- Building **window functions** in Spark SQL (LAG, RANK, nested SUM OVER)
- Connecting a BI tool to a data pipeline output end-to-end

---

## 🔮 Future Improvements

- [ ] Add Apache Airflow orchestration to schedule daily runs
- [ ] Implement incremental loads using Delta Lake `MERGE`
- [ ] Add data quality checks with Great Expectations
- [ ] Connect Tableau Desktop directly to Databricks SQL Warehouse
- [ ] Deploy pipeline on AWS with Terraform

---

## 👤 Author

**Your Name**
[LinkedIn](https://linkedin.com/in/yourprofile) · [GitHub](https://github.com/yourusername)
