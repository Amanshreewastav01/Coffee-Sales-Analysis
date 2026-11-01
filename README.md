# ☕ Coffee Shop Sales Analysis

An end-to-end analysis of coffee shop sales data. **MySQL** is used for data cleaning and analysis, and **Power BI** for an interactive dashboard.

The project covers SQL data transformation, KPI calculation (including month-over-month growth with window functions), and dashboard design.

---

## 📂 Project Structure

```
Coffee-Shop-Sales-Analysis/
├── data/
│   └── coffee_shop_sales.csv        # Raw dataset
├── sql/
│   └── coffee_shop_analysis.sql     # Cleaning and analysis queries
├── powerbi/
│   └── Coffee_Shop_Sales.pbix       # Power BI dashboard
└── README.md
```
---

## 🎯 Key Objectives

- Clean and format `transaction_date` and `transaction_time` using SQL.
- Analyze **sales, orders and quantity** across time, products and locations.
- Calculate **Month-over-Month (MoM) growth** using SQL window functions.
- Build an interactive **Power BI dashboard** with:
  - 📅 Calendar heat map
  - 🛍️ Sales by product category and product type
  - 📍 Sales by store location
  - 📈 Daily sales trend with an average line
  - ⏰ Sales by weekday/weekend and by hour
  - 🏆 Top 10 products by sales

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|---|---|
| **MySQL (Workbench)** | Data cleaning and analysis |
| **Power BI** | Dashboard and data storytelling |
| **Excel / CSV** | Raw dataset |

---

## 📊 Dashboard Preview

<img src="https://github.com/user-attachments/assets/6ebcc5e6-a8b7-459d-86d5-fb556a1e8f9d" width="805" height="490" alt="Coffee Shop Sales Dashboard" />

---

## 🔍 SQL Highlights

**Month-over-Month Sales Growth**

```sql
WITH monthly_sales AS (
    SELECT
        MONTH(transaction_date)                          AS month_number,
        ROUND(SUM(transaction_qty * unit_price), 2)      AS total_sales
    FROM coffee_shop_sales
    GROUP BY MONTH(transaction_date)
)
SELECT
    month_number,
    total_sales,
    ROUND(
        (total_sales - LAG(total_sales) OVER (ORDER BY month_number))
        / NULLIF(LAG(total_sales) OVER (ORDER BY month_number), 0) * 100,
        2
    ) AS mom_growth_pct
FROM monthly_sales
ORDER BY month_number;
```

The first month has no previous month, so its growth value is `NULL`.

---

## 📈 Key Insights

- **Growth:** May recorded higher sales than April, showing positive MoM growth.
- **Weekdays vs weekends:** Weekends outperform weekdays in sales volume.
- **Product mix:** Coffee is the top-selling product category.
- **Peak hours:** Morning hours (8–10 AM) drive the highest sales, reflecting customer behavior.
- **Locations:** Downtown outlets are the top performers.

---

## 🚀 How to Use

1. Load `coffee_shop_sales.csv` into MySQL as the `coffee_shop_sales` table.
2. Run the queries in `sql/coffee_shop_analysis.sql` to clean the data and reproduce the analysis.
3. Open `powerbi/Coffee_Shop_Sales.pbix` in Power BI Desktop and refresh the data source.

