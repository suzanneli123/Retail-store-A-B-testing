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

After cleaning, the data has 264,833 rows covering 72,636 loyalty customers across 272 stores, from July 2018 to June 2019 (12 months). 

Pre-trial data is from July 2018 - January 2019. Trial data is from February 2019 - April 2019. Post-trial data is from May 2019 - June 2019.

# Methodology

**Tools:** Python (pandas, NumPy, SciPy, matplotlib) and SQL (SQLite, run in memory from inside the Juypter notebook)\

**Approach:**  
1. **Data Cleaning & Preparation**: Checked for duplicates, missing values, and outliers, removed duplicate value, flagged outliers, and prepared the data for analysis.

2. **Exploratory Data Analysis (EDA)**: Used SQL to calculate key sales KPIs and explore monthly sales trends, store performance, and top-selling products and brands.

3. **Customer Analysis**: Analyzed customer purchasing behavior by lifestage, premium tier, and RFM segmentation to identify high-value customers and spending patterns.

4. **Control Store Matching**: Used pre-trial sales and customer trends to identify the most similar control store for each trial store using correlation and similarity scores.

5. **A/B Testing & Statistical Analysis**: Calculated estimated sales lift, performed paired t-tests to evaluate statistical significance, and examined changes in customer counts and purchase frequency.

6. **Business Insights & Recommendations**: Used the key findings to recommend ways to improve customer retention, increase sales, and determine whether the trial should be expanded to other stores.


## Exploratory Data Analysis (EDA) 
???
??
??

# Results
- All three trial stores sold more than expected during the trial. Combined, they were **13.7% above** what their matched control stores predicted.
- Store 77 had the biggest jump (**+26%**). Stores 86 and 88 were both around **+12 to 13%**.
- **None of the differences were statistically significant** at the 0.05 level (p-values between 0.08 and 0.11). The numbers point in the right direction, but there's too much day-to-day noise to say the trial *caused* the increase.
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
