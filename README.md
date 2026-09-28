# Techno Sales - Exploratory Data Analysis Report

**Dataset:** `Complete_Techno_Sales_Data.xlsx` | **Period:** 11 Jan 2020 - 31 Dec 2022 | **Tools:** Python (pandas, NumPy, Matplotlib, Seaborn)
**Companion files:** `notebooks/01_techno_sales_eda.ipynb` (code + outputs), `docs/DATA_DICTIONARY.md`

---

## Contents
1. Executive summary
2. Objectives and scope
3. Data description
4. Data quality audit
5. Cleaning and feature engineering
6. Univariate analysis
7. Business KPIs
8. Time-series and seasonality
9. Product and category analysis
10. Brand analysis
11. Geographic analysis
12. Supervisor performance
13. Order status and pipeline
14. Customer analysis
15. Correlation and bivariate analysis
16. Conclusions and recommendations
17. Assumptions and limitations

---

## 1. Executive summary

The business sold **99.3M** of computer hardware (assumed INR) across 5,095 order lines at a fixed **23.1% gross margin**, earning **22.9M** gross profit. Revenue has been flat for three years (32.3M, 33.7M, 33.3M).

- **Five categories** (Monitor, CPU, Graphic Card, HDD, SSD) generate ~79% of revenue.
- **Maharashtra** contributes ~18% of revenue and the top three states ~36%.
- **March-May** is the peak season (~63% of annual revenue); ordering is very weak from June to August.
- **71.5% of revenue** sits in orders not marked *Delivered*, and the status split is ~25% per stage even for 2020 orders - the status field is probably not being maintained.
- Several **master-data problems** exist: product names in four categories describe other products, Ryzen CPUs are labelled Intel, and labels are inconsistent.
- The dataset shows signs of being **simulated** (flat markup, uniform behaviour), so findings describe this dataset specifically.

---

## 2. Objectives and scope

Questions addressed:
1. How large is the business and how has it trended?
2. Which categories, brands, states and supervisors drive results?
3. Is demand seasonal?
4. How much revenue is stuck in the order pipeline?
5. Is the data reliable enough for decision-making?

Scope: descriptive and diagnostic analysis only; no forecasting or modelling.

---

## 3. Data description

| Sheet | Rows | Columns | Role |
|---|---|---|---|
| `Sales_Data` | 5,095 | 14 | Fact table, one row per order line |
| `State_list` | 35 | 2 | State code to name lookup |
| `Supervisor` | 6 | 2 | Supervisor name and image link |

`Sales_Data` has 5 numeric columns (`Cost`, `Sales`, `Quantity`, `Total_Cost`, `Total_Sales`), one date, one identifier and seven categorical columns. Column definitions are in `docs/DATA_DICTIONARY.md`.

| Numeric column | Mean | Median | Min | Max |
|---|---|---|---|---|
| Cost | 6,066 | 6,550 | 350 | 14,500 |
| Sales (unit price) | 7,886 | 8,515 | 455 | 18,850 |
| Quantity | 2.49 | 2 | 1 | 4 |
| Total_Cost | 14,992 | 10,720 | 350 | 58,000 |
| Total_Sales | 19,489 | 13,936 | 455 | 75,400 |

---

## 4. Data quality audit

### 4.1 Integrity checks (all on the raw file)

| Check | Result |
|---|---|
| Missing values | 0 |
| Duplicate rows / duplicate `Order_Number` | 0 / 0 |
| `Total_Cost` = `Cost` x `Quantity` | Holds for all rows |
| `Total_Sales` = `Sales` x `Quantity` | Holds for all rows |
| Distinct `Sales/Cost` ratios | 1 (always 1.30) |
| State codes not in `State_list` | 0 |
| Supervisors not in `Supervisor` sheet | 0 |

### 4.2 Consistency problems

| # | Finding | Severity | Treatment |
|---|---|---|---|
| 1 | `MotherBoard` vs `Motherboard` (2 rows) | Low | Standardised |
| 2 | 1,228 raw product names reduce to 1,082 after fixing case/whitespace (146 duplicated spellings) | Medium | Canonical spelling applied |
| 3 | Typo `Dsipaly` in 150 Monitor rows | Low | Corrected |
| 4 | Product names in **Computer Case**, **NIC** and **Printer** (100% of rows) and **Mouse** (25%) describe RAM or motherboards | **High** | Flagged, left as recorded |
| 5 | Ryzen CPUs (150 rows, ~3.9% of revenue) recorded as brand Intel | Medium | Flagged |
| 6 | Constant 30% markup, uniform status split, repeating monthly volumes, near-uniform customer activity | Info | Noted as sign of simulated data |

Why item 4 was not "fixed": there is no reliable way to tell whether the category or the product name is wrong, so altering either would introduce unverified assumptions.

---

## 5. Cleaning and feature engineering

**Cleaning:** renamed `Assigned Supervisor` to `Supervisor`; standardised `Category`; normalised `Product` spelling; joined state names.

**Features added:** `State_Name`, `Region` (analyst grouping into 7 regions), `Profit`, `Margin_Pct`, `Year`, `Quarter`, `Month`, `Month_Name`, `Year_Month`, `Weekday`, `Is_Weekend`.

**Validation:** row count unchanged (5,095), no missing state or region, 13 categories, output saved to `data/techno_sales_clean.csv` (25 columns).

---

## 6. Univariate analysis

![Numeric distributions](figures/01_numeric_distributions.png)

- Unit `Cost` and `Sales` are multi-modal: cheap peripherals cluster at 350-3,000 and components at 8,000-13,000.
- `Total_Sales` is right-skewed (skewness 0.95); `Quantity` is close to uniform over 1-4 (skewness 0.03).
- **Outliers:** 29 rows (0.6%) exceed the IQR fence of 65,812. All are CPU orders at high quantity - genuine large orders, kept.

![Categorical counts](figures/02_categorical_counts.png)

- Monitor (749 lines) and CPU (600) and Mouse (599) have the most order lines.
- Order lines are split almost evenly across the four statuses (~1,270 each).
- Samsung (1,199 lines) and Dell (898) are the most frequent brands.
- Supervisors handle 791-967 order lines each.

---

## 7. Business KPIs

| KPI | Value |
|---|---|
| Revenue | 99,298,043 |
| Cost | 76,383,110 |
| Gross profit | 22,914,933 |
| Gross margin | 23.08% |
| Order lines | 5,095 |
| Units sold | 12,671 |
| Average / median order value | 19,489 / 13,936 |
| Customers | 56 |
| States / UTs | 35 |
| Categories / brands | 13 / 11 |

---

## 8. Time-series and seasonality

| Year | Revenue | Profit | Order lines | Units | YoY revenue | AOV |
|---|---|---|---|---|---|---|
| 2020 | 32,319,599 | 7,458,369 | 1,646 | 4,142 | - | 19,635 |
| 2021 | 33,657,455 | 7,767,105 | 1,717 | 4,276 | +4.1% | 19,602 |
| 2022 | 33,320,989 | 7,689,459 | 1,732 | 4,253 | -1.0% | 19,238 |

![Monthly revenue](figures/03_monthly_revenue_trend.png)

Revenue is flat. Note that 2020 starts on 11 January and had a lighter December (89 orders vs 146 and 161), so 2021's growth is slightly flattered.

![Seasonality](figures/04_seasonality.png)

| Month | Share of annual revenue |
|---|---|
| Mar | 28.1% |
| Apr | 22.8% |
| May | 12.4% |
| Sep | 9.5% |
| Dec | 7.7% |
| Nov | 4.3% |
| Other months (Jan, Feb, Jun, Jul, Aug, Oct) | 1.8% - 3.5% each |

March-May combined = **63.3%**. The pattern repeats nearly identically each year (for example March has 470/471/471 orders in 2020/21/22).

![Weekday and quarter](figures/05_weekday_quarter.png)

Tuesday (16.5M) and Saturday (16.1M) are the strongest weekdays; Thursday (10.2M) is weakest. Q1 and Q2 together account for roughly 70-73% of revenue every year.

---

## 9. Product and category analysis

| Category | Revenue | Share | Avg unit price | Units |
|---|---|---|---|---|
| Monitor | 23,297,105 | 23.5% | 12,558 | 1,843 |
| CPU | 18,760,300 | 18.9% | 13,000 | 1,450 |
| Graphic Card | 13,113,100 | 13.2% | 11,483 | 1,155 |
| HDD | 12,886,250 | 13.0% | 11,480 | 1,119 |
| SSD | 10,191,350 | 10.3% | 9,317 | 1,103 |
| Mouse | 3,831,893 | 3.9% | 2,548 | 1,496 |
| Motherboard | 3,163,329 | 3.2% | 8,099 | 385 |
| RAM | 3,154,697 | 3.2% | 4,264 | 743 |
| Cabinet | 2,947,594 | 3.0% | 2,569 | 1,140 |
| Printer | 2,873,052 | 2.9% | 8,099 | 357 |
| Computer Case | 1,917,994 | 1.9% | 5,083 | 379 |
| NIC | 1,771,484 | 1.8% | 5,083 | 359 |
| Keyboard | 1,389,895 | 1.4% | 1,213 | 1,142 |

![Category Pareto](figures/06_category_revenue_pareto.png)

- Five categories = 78.8% of revenue; six reach the 80% mark.
- Low-priced items (Mouse, Keyboard, Cabinet) move many units but earn little; Monitors and CPUs combine high volume with high price.

![Price vs volume](figures/07_category_price_volume.png)

**Growth 2020 to 2022 (CAGR):** NIC +8.2%, HDD +5.6%, Motherboard +4.2%, SSD +3.5%; declining: Printer -3.1%, CPU -1.8%, Mouse -1.3%. Revenue changes are small in absolute terms.

Top products by revenue: 2GB Graphic Card (7.2M), I7 - Intel 12th Generation (6.6M), 26" LCD Display (6.6M).

*Caution:* Computer Case, NIC, Printer and part of Mouse carry mismatched product names (section 4).

---

## 10. Brand analysis

| Brand | Revenue | Share | Categories |
|---|---|---|---|
| Intel | 18,760,300 | 18.9% | 1 |
| Samsung | 16,166,345 | 16.3% | 4 |
| Dell | 14,235,195 | 14.3% | 2 |
| Nvidia | 13,113,100 | 13.2% | 1 |
| Western Digital | 8,050,250 | 8.1% | 1 |
| Acer | 6,558,630 | 6.6% | 1 |
| Gigabyte | 5,886,855 | 5.9% | 3 |
| Hynix | 5,538,520 | 5.6% | 3 |
| Seagate | 4,836,000 | 4.9% | 1 |
| MSI | 3,205,254 | 3.2% | 3 |
| Asus | 2,947,594 | 3.0% | 1 |

![Brand analysis](figures/08_brand_analysis.png)

Each brand is almost entirely tied to specific categories, so brand performance largely mirrors category performance. Intel's share includes ~3.9% of revenue that is actually Ryzen (AMD) products.

---

## 11. Geographic analysis

![Geography](figures/09_geography.png)

| State | Revenue | Share |
|---|---|---|
| Maharashtra | 17,621,084 | 17.8% |
| Uttar Pradesh | 9,264,645 | 9.3% |
| Gujarat | 9,137,726 | 9.2% |
| Delhi | 5,061,953 | 5.1% |
| Bihar | 4,862,221 | 4.9% |
| Tripura | 3,660,657 | 3.7% |
| Tamil Nadu | 3,428,763 | 3.5% |
| Chandigarh | 1,958,697 | 2.0% |

- Top 3 states = 36.3%; bottom 5 states = 6.5%.
- **Region view:** West 31.7M (5 states), North 26.2M (9), North-East 15.5M (8), East 10.2M (4), South 9.8M (5), Central 3.6M (2), Islands 2.3M (2).
- West earns 6.3M per state versus 1.1M-2.9M elsewhere.

![State category mix](figures/10_state_category_mix.png)

---

## 12. Supervisor performance

| Supervisor | Revenue | Share | Order lines | AOV | Delivered % |
|---|---|---|---|---|---|
| Aarvi Gupta | 18,685,368 | 18.8% | 967 | 19,323 | 25.5 |
| Ajay Sharma | 17,801,186 | 17.9% | 948 | 18,778 | 25.6 |
| Vijay Singh | 15,939,950 | 16.1% | 794 | 20,076 | 24.3 |
| Roshan Kumar | 15,887,079 | 16.0% | 798 | 19,909 | 23.8 |
| Aadil Khan | 15,730,767 | 15.8% | 797 | 19,737 | 25.7 |
| Advika Joshi | 15,253,693 | 15.4% | 791 | 19,284 | 24.0 |

![Supervisor performance](figures/11_supervisor_performance.png)

Every supervisor covers all 35 states and a nearly identical category mix (maximum difference in any category share: 3.5 percentage points). Aarvi Gupta and Ajay Sharma handle ~20% more order lines than the others; Vijay Singh has the highest average order value.

---

## 13. Order status and pipeline

| Status | Order lines | Revenue | Revenue share |
|---|---|---|---|
| Order | 1,276 | 28,620,410 | 28.8% |
| Processing | 1,278 | 21,233,992 | 21.4% |
| Shipped | 1,273 | 21,103,186 | 21.3% |
| Delivered | 1,268 | 28,340,455 | 28.5% |

![Status pipeline](figures/12_status_pipeline.png)

**71.5% of revenue (70.96M)** is not in Delivered status, and the ~25% split per stage is the same for orders placed in 2020, 2021 and 2022. Orders that old should be almost fully delivered, so the status field appears not to be updated.

---

## 14. Customer analysis

![Customers](figures/13_customers.png)

- 42 repeat customers place 120-122 orders each and average 2.36M revenue.
- **14 customers (25%) bought exactly once**, all between 18 and 26 March 2020, contributing 0.23% of revenue.
- Revenue per repeat customer ranges only from 1.81M to 2.97M; the top-10 customers hold 28.2% of revenue.
- Every repeat customer buys from 34-35 different states, which is implausible for real retail buyers and supports the simulated-data reading.

---

## 15. Correlation and bivariate analysis

![Correlation](figures/14_correlation_and_order_value.png)

- `Cost` and `Sales` are perfectly correlated (r = 1.0) because of the fixed markup.
- `Quantity` vs `Total_Sales`: r = 0.52; unit price vs quantity: r = -0.02 - order size is unrelated to price.
- Order value is driven by category price and quantity, not by any relationship between the two.

![Quantity by category](figures/15_quantity_by_category.png)

---

## 16. Conclusions and recommendations

**Conclusions**
1. Stable but non-growing business; revenue ~33M per year.
2. Revenue and profit are concentrated in five categories, three states and one quarter-and-a-half of the year.
3. Team performance is balanced; no supervisor stands out on mix or delivery.
4. Data quality issues limit category, brand and status conclusions.

**Recommendations**
1. Correct master data (category/product/brand mapping, spelling) at source.
2. Audit and enforce status updates; report delivery lead time once reliable.
3. Plan inventory and staffing for March-May; build promotions for June-August.
4. Explore growth in low-share regions (Islands, Central, South) before committing spend.
5. Run a win-back campaign for the 14 one-time customers.
6. Record real product-level costs to allow true margin analysis.

---

## 17. Assumptions and limitations
- Currency assumed INR; `Total_Sales` treated as revenue; gross profit excludes discounts, returns and overheads.
- `Region` is an analyst-defined grouping, not source data.
- All orders count as sales regardless of status (no cancelled or returned status exists).
- The data shows signs of being simulated; patterns may not reflect real-world behaviour.
