# 🏷️ Gender-Based Sales KPI Dashboard — Azure End-to-End Data Engineering

![Azure](https://img.shields.io/badge/Azure-Data%20Engineering-0078D4?style=for-the-badge&logo=microsoftazure)
![Python](https://img.shields.io/badge/Python-3.10-3776AB?style=for-the-badge&logo=python)
![SQL](https://img.shields.io/badge/SQL%20Server-2022-CC2927?style=for-the-badge&logo=microsoftsqlserver)
![Power BI](https://img.shields.io/badge/Power%20BI-Dashboard-F2C811?style=for-the-badge&logo=powerbi)

## 📋 Business Request

The company identified a gap in understanding customer demographics — specifically **gender distribution** and how it influences product purchases. Stakeholders requested a comprehensive KPI dashboard showing:
- Total products sold by gender and product category
- Total sales revenue by gender
- Clear gender split among customers
- Filters by product category, gender, and date range

## 🏗️ Architecture

```
On-Prem SQL Server (AdventureWorksDW2022)
        │
        ▼
Azure Data Factory ──► Azure Data Lake Gen2
                              │
                    ┌─────────┴──────────┐
                 Bronze              Databricks
                 (Raw)           (Transform/Aggregate)
                    │                   │
                 Silver              Silver
                 (Clean)             Gold
                    │                   │
                  Gold ────────────────►│
                    │
                    ▼
          Azure Synapse Analytics
                    │
                    ▼
               Power BI Dashboard
                    
Security: Azure Key Vault + Azure Active Directory
```

## 🛠️ Technologies Used

| Technology | Purpose |
|---|---|
| SQL Server 2025 Express | On-premises source database |
| Azure Data Factory | Pipeline orchestration & extraction |
| Azure Data Lake Storage Gen2 | Bronze/Silver/Gold storage |
| Azure Databricks (Trial) | PySpark transformations |
| Azure Synapse Analytics | SQL serverless query layer |
| Azure Key Vault | Secrets management |
| Power BI | Interactive KPI dashboard |

## 📂 Project Structure

```
azure-gender-sales-dashboard/
├── README.md
├── sql/
│   ├── create_adf_user.sql
│   └── exploratory_queries.sql
├── notebooks/
│   ├── 02_bronze_to_silver.ipynb
│   └── 03_silver_to_gold.ipynb
├── adf/
│   └── pipeline_export.json
└── powerbi/
    └── gender_sales_dashboard.pbix
```

## 🔄 Pipeline — Bronze → Silver → Gold

**Bronze:** Raw Parquet files copied from SQL Server via ADF  
**Silver:** Cleaned data (Gender normalized M→Male/F→Female, types cast, duplicates removed)  
**Gold:** KPI-ready aggregated tables (revenue by gender+category, gender split %, monthly trend)

## 📊 Key SQL Query

```sql
SELECT
    c.Gender,
    SUM(f.SalesAmount) AS TotalRevenue,
    ROUND(100.0 * SUM(f.SalesAmount) / SUM(SUM(f.SalesAmount)) OVER(), 1) AS PctRevenue
FROM FactInternetSales f
JOIN DimCustomer c ON f.CustomerKey = c.CustomerKey
GROUP BY c.Gender;
```

## 👤 Author

**Hicham ERRIHANI** — Data Scientist / Data Engineer  
[LinkedIn](https://www.linkedin.com/in/hicham-errihani) | [GitHub](https://github.com/Hicham-Errihani)
