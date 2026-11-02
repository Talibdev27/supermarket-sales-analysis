# Supermarket Sales Analysis

Practising data analysis with pandas on the [Supermarket Sales](https://www.kaggle.com/datasets/lovishbansal123/sales-of-a-supermarket) dataset: 1,000 transactions from 3 branches in Myanmar, January–March 2019.

## Progress

**Level 1: Cleaning (done)**
- Converted `Date` from text to datetime and checked the range (2019-01-01 to 2019-03-30)
- Added `month`, `weekday` and `hour` columns
- Verified the money columns: `cogs = unit price × quantity` and `total = cogs × 1.05`
- Dropped redundant columns: `gross income` (copy of `Tax 5%`) and `gross margin percentage` (constant)
- Renamed columns to snake_case

**Findings so far**
- Busiest day is Saturday, busiest hour is 7 PM; the shop trades roughly 10 AM–9 PM
- February has the fewest sales, but it also has the fewest days

**Next:** Level 2, aggregation with `groupby`

## Run it

```bash
pip install pandas matplotlib kagglehub jupyter
python -c "import kagglehub, shutil; p = kagglehub.dataset_download('lovishbansal123/sales-of-a-supermarket'); shutil.copytree(p, 'data', dirs_exist_ok=True)"
jupyter notebook practice.ipynb
```
