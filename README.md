# Supermarket Sales Analysis

Practising data analysis with pandas on the [Supermarket Sales](https://www.kaggle.com/datasets/lovishbansal123/sales-of-a-supermarket) dataset: 1,000 transactions from 3 branches in Myanmar, January–March 2019.

## Summary for the store manager

1. **Staffing:** The busiest times are Tuesday and Saturday at 7 PM (about 9,000 revenue each), while Monday evenings are quiet. → Add extra cashiers on Tuesday and Saturday evenings; use fewer staff on Monday evenings.
2. **Stock:** Saturday, Tuesday and Sunday bring in the most money per day (Saturday about 4,300/day), and Monday the least (about 3,150/day). → Restock shelves before Saturday and Tuesday.
3. **Branches:** Naypyitaw earns the most (110,569) with the fewest customers, because each spends more (337 per visit). Yangon has the most customers (340) but the smallest baskets (312). → Naypyitaw: advertise to bring in more customers. Yangon: train staff to suggest extra products and try bundle offers.
4. **Loyalty program:** Members spend about 3% more, but a t-test (p = 0.53) shows this could be luck. → Don't invest more in the program yet; test it properly first.
5. **Payments:** Naypyitaw prefers cash (124 sales) and uses cards least (98); Yangon prefers Ewallet (126). → Keep enough cash handling in Naypyitaw and make Ewallet fast and reliable in Yangon.
6. **Health and beauty:** This is the lowest-revenue product line (49,194) because it has the fewest sales (152), not because baskets are small (average 324, about the same as other lines). → Promote it more, for example with better shelf placement or discounts, to bring in more buyers.

**Limitations**
- The data covers only 3 months (Jan–Mar 2019), so seasonal patterns (holidays, summer) can't be seen.
- There is no cost or profit data: "gross income" is just the 5% tax, so this analysis is about revenue, not profit.
- Some differences are small (members vs. normal, ratings) and were not statistically significant, so they should be tested before acting on them.

## How I got there

The full analysis is in [practice.ipynb](practice.ipynb), in four levels.

**Level 1: Cleaning (done)**
- Converted `Date` from text to datetime and checked the range (2019-01-01 to 2019-03-30)
- Added `month`, `weekday` and `hour` columns
- Verified the money columns: `cogs = unit price × quantity` and `total = cogs × 1.05`
- Dropped redundant columns: `gross income` (copy of `Tax 5%`) and `gross margin percentage` (constant)
- Renamed columns to snake_case

Findings:
- Busiest day is Saturday, busiest hour is 7 PM; the shop trades roughly 10 AM–9 PM
- February has the fewest sales, but it also has the fewest days

**Level 2: Aggregation with `groupby` (done)**
- Revenue, transaction count and average sale per city and per product line
- Payment method per city, members vs. normal customers, average rating per product line and city

Findings:
- **Naypyitaw leads revenue (110.6k) despite the fewest transactions (328).** Its average sale (337) is about 8% above Yangon's (312), the busiest branch.
- **Food and beverages is the top product line** by balancing volume and basket size, without leading either one. Health and beauty is last because it has the fewest transactions (152), not small baskets.
- Basket size in items is flat across product lines (about 5–6 items), so revenue differences come from transaction counts and prices.
- **The overall payment split hides city differences:** Ewallet and Cash are tied overall, but Yangon prefers Ewallet, Naypyitaw prefers Cash, and Mandalay is evenly split.
- Members spend about 3% more per visit (327.79 vs. 318.12). The gap is small and not statistically significant (see Level 4).
- Ratings are similar everywhere (about 6.8–7.1). Home and lifestyle and Mandalay are the lowest.

**Level 3: Time trends and charts (done)**
- Daily revenue line chart with a 7-day rolling average
- Revenue by hour and weekday as bar charts, including revenue per day to correct for unequal day counts
- Pivot tables: gender × product line and month × branch, with month-over-month growth
- Weekday × hour revenue heatmap

Findings:
- **Revenue is flat across Q1.** Daily revenue is noisy, and the 7-day average shows no growth, with a dip in late February and a recovery in early March.
- **Correcting for day counts changes the picture.** Monday and Sunday occur 12 times in the period and the other weekdays 13 times. Per day, Sunday moves from 4th to 3rd, and Monday is still the weakest day.
- **Hour 14 has few sales (83) but the highest average sale of any hour (371).**
- **Men drive Health and beauty revenue** (30.6k vs. 18.6k for women), against the usual expectation. Women lead Food and beverages (33.2k vs. 23.0k). Level 4 shows that gender and product choice are not significantly linked overall.
- **Branch B did not dip in February.** In raw totals all branches fell from January to February, but per day Branch B grew 2.5%; only A (−14.5%) and C (−9.8%) really dipped. B was also the only branch that fell per day in March.
- **The 7 PM peak is mainly Tuesday and Saturday** (about 9.2k and 9.1k). On Monday, 7 PM is one of the quietest slots. The quietest slot overall is Friday 5 PM.

**Level 4: Statistics and SQL (done)**
- t-test: members vs. normal customers
- Correlation between rating, total, unit price and quantity
- Chi-square test: gender vs. product line
- The Level 2 groupby questions rewritten in SQL (SQLite)

Findings:
- **The members' 3% lead is not significant** (t-test p = 0.53), so the loyalty program is not shown to raise spending.
- **Rating is not linked to spend** (correlation −0.04). Total correlates with quantity (0.71) and unit price (0.63) only because it is calculated from them.
- **Gender and product choice are not significantly linked** (chi-square p = 0.33), even though Health and beauty has 88 male vs. 64 female sales.
- The SQL queries return the same numbers as the pandas versions.

## Run it

```bash
pip install pandas matplotlib scipy kagglehub jupyter
python -c "import kagglehub, shutil; p = kagglehub.dataset_download('lovishbansal123/sales-of-a-supermarket'); shutil.copytree(p, 'data', dirs_exist_ok=True)"
jupyter notebook practice.ipynb
```
