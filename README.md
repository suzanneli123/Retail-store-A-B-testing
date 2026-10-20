# Retail Store Trial: A/B Testing & Customer Analytics

# Business Goal
A supermarket retailer wants to increase chip sales while gaining a better understanding of customer purchasing behaviour.
As part of this initiative, the retailer introduced a **new store layout in three trial stores** and wanted to determine whether the change resulted in a meaningful improvement in sales.

The goal of this project
1. Understand overall chip sales performance and purchasing trends
2. Identify important customer segments
3. Measure the sales impact of the new store layout
4. Test whether the observed sales uplift is statistically significant

To achieve this, I analyzed transaction data to identify sales trends and customer purchasing patterns. I used RFM segmentation to group customers based on their shopping behavior and A/B testing to compare the performance of three trial stores with similar control stores. The findings were used to develop recommendations to help the retailer improve customer retention, increase sales, and decide whether to expand the new layout to other stores.

# Data Source


There are three files. They come from Quantium retail analytics virtual experience (the "QVI" datasets):

| File | What's in it |
|---|---|
| `QVI_transaction_data.xlsx` | One row per product line in a transaction: date, store, loyalty card, product, quantity, sales |
| `QVI_purchase_behaviour.csv` | One row per loyalty customer: life stage and premium tier (Budget / Mainstream / Premium) |
| `QVI_data.csv` | The two above already merged, plus pack size and brand.  **This is the one the analysis runs on.** |

### Data Source after cleaning
- **264,833 transaction records**
- **72,636 loyalty customers**
- **272 stores**
- **12 months of data**
- Analysis period: **July 2018 to June 2019**

## Trial Timeline
| Period | Dates | Purpose |
|---|---|---|
| Pre-Trial | Jul 2018 – Jan 2019 | Used to understand historical performance and identify control stores |
| Trial | Feb 2019 – Apr 2019 | New store layout evaluated |
| Post-Trial | May 2019 – Jun 2019 | Used to observe performance after the experiment |

# Methodology

**Tools:** Python (pandas, NumPy, SciPy, matplotlib) and SQL (SQLite run in from inside the Juypter notebook)

**Approach:**  
1. **Data Cleaning & Preparation**: Checked for duplicates, missing values, and outliers, removed duplicate value, flagged outliers, and prepared the data for analysis.

2. **Exploratory Data Analysis (EDA)**: Used SQL to evaluate key sales KPIs, monthly trends, store performance, top-performing products and brands, and customer purchasing behaviour through RFM segmentation.
   
3. **Control Store Matching**: Used pre-trial sales and customer trends, along with correlation and similarity scores, to identify **a matched control store** for each trial store. The matched control represents a non-trial store with similar historical performance and serves as the benchmark for measuring the impact of the intervention.

4. **A/B Testing & Statistical Analysis**: Calculated estimated sales lift, performed paired t-tests to evaluate statistical significance, and examined changes in customer counts and purchase frequency.

5. **Business Insights & Recommendations**: Used the key findings to recommend ways to improve customer retention, increase sales, and determine whether the trial should be expanded to other stores.

## Exploratory Data Analysis (EDA) 

**KPIs**

Average sales per transaction
```sql
SELECT 
    AVG(transaction_sales) AS avg_sales_per_transaction
FROM (
    SELECT 
        TXN_ID,
        SUM(TOT_SALES) AS transaction_sales
    FROM sales
    GROUP BY TXN_ID
);
```

Average quantity per transaction
```sql
SELECT
    AVG(quantity) AS avg_quantity_per_transaction
    FROM (
      SELECT
        TXN_ID,
        SUM(PROD_QTY) AS quantity
    FROM sales
    GROUP BY TXN_ID);
```


Average transactions per month
```sql
   SELECT
    AVG(monthly_transactions) AS avg_transactions_per_month
   FROM (
    SELECT
        YEAR_MONTH,
        COUNT(DISTINCT TXN_ID) AS monthly_transactions
    FROM sales
    GROUP BY YEAR_MONTH);
```

Average transactions per store
```sql
SELECT
    AVG(store_transactions) AS avg_transactions_per_store
FROM (
    SELECT
        STORE_NBR,
        COUNT(DISTINCT TXN_ID) AS store_transactions
    FROM sales
    GROUP BY STORE_NBR);
```

Monthly sales
```sql
   SELECT
    YEAR_MONTH,
    SUM(TOT_SALES) AS total_sales
  FROM sales
  GROUP BY YEAR_MONTH
  ORDER BY YEAR_MONTH;
```


#### Figure 1: Monthly Sales Trend
Analysis of total sales aggregated by year and month.
<p align="center">
  <img src="https://github.com/user-attachments/assets/488a64dd-b636-4154-aded-e9f95ec95483" width="700">
</p>

#### Figure 2: Top 5 Chip Brands by sales
Analysis of top 5 chip brands.
<p align="center">
  <img src="https://github.com/user-attachments/assets/bb326be0-c279-4e91-9d28-b30d519c4306" width="700">
</p>

#### Figure 3: Customer RFM segmentation (Recency, Frequency, Monetary Value) with Sales
Analysis of Customer groups based on RFM and sales
<p align="center">
  <img src="https://github.com/user-attachments/assets/f3e4023f-d5f8-4fad-a49c-1f8d65eadf91" width="700">
</p>

#### Figure 4: Visual analysis of Customer RFM segmentation (Recency, Frequency, Monetary Value) with Sales
<p align="center">
  <img src="https://github.com/user-attachments/assets/5bc9a089-e34d-4569-a175-dd0d190522f0" width="700">
</p>

## A/B testing

#### Figure 5: Matched control store and Trial store using similarity score
<p align="center">
  <img src="https://github.com/user-attachments/assets/3d4afd22-11e7-42c4-aeac-4ea19b2ebccf" width="700">
</p>

#### Figure 6: Monthly sales comparison of Trial store vs. Matched Control store
<p align="center">
  <img src="https://github.com/user-attachments/assets/1d78cd4b-a614-467a-bbdb-0ab3155cf025" width="700">
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/92e9705a-6fac-467b-90b3-6fcb5340897e" width="700">
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/c800261b-5c8d-46f1-8184-732b4d5f7679" width="700">
</p>

#### Figure 7: Paired T-Test of Trial store vs. Matched Control store
<p align="center">
  <img src="https://github.com/user-attachments/assets/1ba65d4f-63ef-43c2-9f20-a0326b17d5ea" width="700">
</p>


#### Figure 8: Statistical Significance of Paired T-test results (Trial store vs. Matched Control store)
<p align="center">
  <img src="https://github.com/user-attachments/assets/e986df57-522a-415b-abfb-65c85c1d6d32" width="700">
</p>

# Results
- All three trial stores sold more than expected during the trial. Combined, they were **13.7% above** what their matched control stores predicted.
- Store 77 had the biggest jump (**+26%**). Stores 86 and 88 were both around **+12 to 13%**.
- **None of the differences were statistically significant** at the 0.05 level (p-values between 0.08 and 0.11). We fail to reject the null hypothesis and conclude that the test did not provide enough evidence to conclude that average daily sales were different during the trial.
- At stores 77 and 86 the extra sales came mostly from **more customers**, not from people shopping more often. At store 88 it was a bit of both.

# Recommendations 
- **Don't roll out company-wide yet.** The results look promising, but the evidence isn't strong enough. Extend the trial or add more stores so the test has enough statistical power.
- **Marketing:** try loyalty offers for High Value customers and win-back offers for the At Risk group. Together they make up over 80% of sales.
- **Supply chain:** if the trial does expand, check stock levels and reorder plans for Kettle and the other top brands first. More customers coming in only helps if the shelves aren't empty.
- **Store operations:** find out what store 77 did differently (displays, staffing, how the trial was run). It had nearly double the lift of the other two stores.

# Limitations

- **Small sample.** Three stores over three months. Having more stores and over longer period would help provide more reliable results.
- **Daily sales aren't independent.** The paired t-test assumes they are, and they probably aren't (weekly patterns, for one). A permutation test or a difference-in-differences model would be a good way to check the result.
- **Store 88's control is weak.** I'd like to try matching on more metrics, or use a synthetic control built from several stores instead of just one.
- **External factors** Some of the change could come from pricing, promotions, and product mix, not the trial itself.

# Challenges
- **Control Store Matching**: Finding stores with similar sales performance and customer patterns to make fair comparisons.

# 🧠 Key Takeaway
> **Good analytics is not about proving that an idea worked. It is about determining whether the available evidence is strong enough to support a business decision.**
This trial generated promising results, but responsible decision-making requires additional evidence before committing to a large-scale rollout.
---
# 💼 Skills Demonstrated
This project demonstrates practical experience in:
`Python` • `SQL` • `Pandas` • `NumPy` • `SciPy` • `Matplotlib` • `EDA` • `RFM Segmentation` • `Customer Analytics` • `Experimental Analysis` • `Statistical Testing` • `Control Store Matching` • `Business Analytics` • `Data Storytelling`
---
## 👤 Author
**Suzanne Li**
Retail Analytics | Customer Analytics | Experimentation | Python | SQL
