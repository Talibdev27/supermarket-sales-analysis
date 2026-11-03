# Supermarket Sales Analysis

Practising data analysis with pandas on the [Supermarket Sales](https://www.kaggle.com/datasets/lovishbansal123/sales-of-a-supermarket) dataset: 1,000 transactions from 3 branches in Myanmar, January–March 2019.

## Progress

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
- Members spend about 3% more per visit (327.79 vs. 318.12). The gap is small and still needs a significance test (Level 4).
- Ratings are similar everywhere (about 6.8–7.1). Home and lifestyle and Mandalay are the lowest.

**Next:** Level 3, time trends and charts

## Run it

```bash
pip install pandas matplotlib kagglehub jupyter
python -c "import kagglehub, shutil; p = kagglehub.dataset_download('lovishbansal123/sales-of-a-supermarket'); shutil.copytree(p, 'data', dirs_exist_ok=True)"
jupyter notebook practice.ipynb
```
