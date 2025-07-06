
# Amazon eCommerce Sales Dashboard 📊

This Power BI project analyzes real sales, returns, and profitability from Amazon commerce activity between May 2024 and May 2025. Built with SQL, Excel, and DAX, the dashboard delivers operational insights for product performance and margin optimization.

---

## 🚀 Project Highlights

- 📈 Total Sales Analyzed: $164,800+ across 3,135 orders
- 💰 Total Profit: $40,340 (24% profit margin)
- 📉 Return Rate: 4.98%, which is below the industry benchmark for e-commerce
- 📍 Top-selling regions: Ontario & Quebec
- 🧦 Nearly 60% of total sales came from the Socks category, making it the top-performing product line.
- 🧠 Product-level insights identified key listing/image issues

---
## 🛠 Tools Used

- Power BI — DAX modeling, Power Query transformations, and dashboard design  
- SQL (MySQL) — Pre-cleaned refunds and returns data  
- Excel — Removed unnecessary rows/columns from raw CSVs  
- Power Query — Cleaned and reshaped transactions and listings within Power BI  
- Amazon Reports — Real commerce data: transactions, returns, refunds, listings


## 📄 Project Preview

- 📥 [Download the full PDF report](https://github.com/Hikmatguliyev05/amazon-ecommerce-performance-report/blob/main/Project%20Pdf.pdf)  
  - Page 1: **Executive Summary**  – Key KPIs, business performance overview  
  - Page 2: **Revenue Analysis** – Top products, profit margins, regional breakdown  
  - Page 3: **Refund & Returns Analysis** – Damaged return rate, root causes, return trends  
- ▶️ [Watch the dashboard walkthrough (video)](https://github.com/Hikmatguliyev05/amazon-ecommerce-performance-report/blob/main/Project%20Recording.mp4)

---

## 🖼️ Dashboard Screenshots

### 📌 Executive Summary  
![Executive Summary](https://github.com/Hikmatguliyev05/amazon-ecommerce-performance-report/blob/main/Executive%20Summary.png)

### 📌 Revenue Analysis  
![Revenue Analysis](https://github.com/Hikmatguliyev05/amazon-ecommerce-performance-report/blob/main/Revenue%20Analysis.png)

### 📌 Refund & Returns Analysis  
![https://github.com/Hikmatguliyev05/amazon-ecommerce-performance-report/blob/main/refund%20and%20returns.png)

```

## 🔑 Key Insights

- 🛠 Damaged returns cause direct business loss
- 🧾 Damaged Return Rate: **1.05%**, well below ~2% industry benchmark
- 📸 High-return item traced back to misleading product images
- 📦 Ontario drives nearly 50% of total revenue
- 🧮 Backend fees visualized to isolate true net profit

---

## 🧹 SQL Cleaning & Preprocessing

### Pre-Cleaning in Excel
- Removed unnecessary columns from the raw CSV files  before importing into Power BI to ensure clean schema alignment.

### Refunds Cleaning
```sql
SELECT
  п»їDate AS Refund_date,
  TRIM(transaction_type) AS transaction_type,
  TRIM(Order_ID) AS order_id,
  TRIM(Product_Details) AS product_description,
  ABS(Total_product_charges) AS product_charges,
  total_promotional_rebates,
  amazon_fees,
  ABS(Other) AS other_fees,
  ABS(Total) AS total_refund
FROM refund;
````

### Returns Cleaning

```sql
SELECT
  LEFT(return_date, 10) AS return_date,
  order_id,
  sku,
  asin,
  fnsku,
  TRIM(product_name) AS product_name,
  quantity,
  LOWER(detailed_disposition) AS detailed_disposition,
  LOWER(reason) AS reason,
  TRIM(status) AS status,
  LOWER(customer_comments) AS customer_comments
FROM return_report;
```

---

## 📐 Data Modeling Approach

* Avoided unreliable “Orders” table due to  canceled orders
* Structured around `Transactions` as fact table
* Linked `Returns`, `Refunds`, and `Listings` as supporting data
* Avoided many-to-many relationships by pre-aggregating in Power Query

---

## 🧮 DAX Measures Created

### Average Order Value

```dax
Average Order Value = 
DIVIDE([Total revenue], [Total Orders])
```

### Damaged Return Rate (%)

```dax
Damaged Return Rate (%) =  
DIVIDE (
    COUNTROWS (
        FILTER (
            Returns,
            Returns[detailed_disposition] IN { "Customer_Damaged", "Defective" }
        )
    ),
    COUNTROWS (Transactions)
)
```

### Lost Revenue from Damaged Returns

```dax
Lost revenue from returned orders =  
CALCULATE (
    SUM ( refunds[total_refund] ),
    FILTER (
        Returns,
        Returns[detailed_disposition] IN { "Customer_Damaged", "Defective" }
    )
)
```

### Most Returned Product

```dax
Most Returned Product = 
MAXX (
    TOPN (
        1,
        SUMMARIZE (
            Returns,
            Returns[Product_Name],
            "ReturnCount", COUNTROWS (Returns)
        ),
        [ReturnCount], DESC
    ),
    Returns[Product_Name]
)
```

### Total Profit

```dax
Total Profit = 
SUMX (
    Transactions,
    Transactions[earningsbeforecogs] - RELATED(listing_report[COGS])
)
```

---

## 🔍 My Project Workflow & Reasoning

This project was built fully from scratch based on raw business data and operational insights.

### Step-by-Step Process:

1. **Initial Cleaning in Excel**

   * Removed unnecessary system-generated columns, blank rows, and irrelevant headers from all 4 raw CSV files.

2. **SQL Cleaning in MySQL**

   * Cleaned and normalized the `refunds` and `returns` tables.
   * Trimmed text, fixed BOM issues, and used `ABS()` for financial values.

3. **Data Import & Power Query Transformations**

   * Imported 4 files: Transactions, Returns, Refunds, and Listings
   * Cleaned `Transactions` and `Listing` again in Power Query
   * Pivoted `transaction_type` to analyze backend fees like inventory & service fee

4. **Grouped by Order ID for Accuracy**

   - Initially detected duplicate `order_id`s in both `Transactions` and `Returns` tables  
   - After investigation, found that Amazon sometimes fulfills a single order through multiple shipments or split returns  
   - Instead of removing duplicates, grouped both tables by `order_id` to ensure accurate aggregation and preserve all associated records

5. **Business Insight Focus**

   * Analyzed damaged returns causing business loss
   * Verified that the 1.05% damaged return rate is below the \~2% industry average
   * Identified top-returned product tied to listing image issues
   * Mapped orders by province to optimize future shipping

---

## 🔒 File Privacy Notice

This dashboard is built using real, private business data.
The `.pbix` file is not shared publicly, but is available **upon request** for serious recruiters or collaborators.
---

## 🙋‍♂️ About Me

I'm **Hikmat**, a data-driven problem solver based in Toronto 🇨🇦

* 🎯 Power BI (PL-300 Certified)
* 📊 Excel Expert
* 💻 Strong SQL + Business Thinking
* 🛒 Turned Amazon commerce data into insight-driven decisions
**Let’s connect on [LinkedIn](https://www.linkedin.com/in/hikmat-guliyev-2b2476329/)**

