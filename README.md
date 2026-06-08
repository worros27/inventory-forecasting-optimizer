# Inventory Forecasting & Weekly Demand Planning Demo

This is a portfolio-safe version of an inventory forecasting workflow. It is designed to demonstrate demand forecasting, intermittent-demand handling, marketplace signal features, and weekly forecast allocation without exposing proprietary company data.

## What this project does

- Loads SKU-level sales history from a CSV file
- Auto-detects the latest sales date and forecasts the next 3.5 months
- Separates non-marketplace demand from marketplace signal rows
- Builds SKU features such as recency, seasonality, spikes, customer concentration, and out-of-stock proxy indicators
- Trains an ML correction layer using historical rolling forecast windows
- Produces target, conservative, and aggressive forecasts
- Breaks the aggressive forecast into seasonal weekly forecast columns
- Exports a final CSV into the `outputs/` folder

## Data safety

The files included in `sample_data/` are synthetic. They do not contain real company sales, customer names, vendor files, SKU IDs, paths, purchase orders, or business rules.

Do not commit private files such as:

- Real sales history
- Real inventory or purchase-order files
- Customer names
- Vendor portal exports
- Internal file paths
- API keys or credentials

## Expected input columns

The default sales file expects these columns:

| Column | Meaning |
|---|---|
| `Part_Number` | SKU or item code |
| `Date` | Sales date |
| `Quantity` | Units sold |
| `Customer` | Customer/channel name |
| `Segment` | Product segment/category |

The target SKU file expects a column named `sku_id`, `SKU`, `Part_Number`, or a first column containing target SKUs.

## Run with sample data

```bash
pip install -r requirements.txt
python inventory_forecasting_portfolio.py
```

## Run with your own approved data

```bash
python inventory_forecasting_portfolio.py \
  --input-file path/to/sales_history.csv \
  --target-sku-file path/to/target_skus.csv \
  --output-dir outputs
```

Optional marketplace forward-signal files are disabled by default. Only enable them with synthetic data or data you are explicitly allowed to use:

```bash
python inventory_forecasting_portfolio.py \
  --input-file path/to/sales_history.csv \
  --target-sku-file path/to/target_skus.csv \
  --use-vendor-forward-signal
```

## Output

The script writes one CSV like:

```text
outputs/forecast_next_3_5_Months_portfolio_forecast_weekly_aggressive_cutoff_YYYY-MM-DD.csv
```

Key output fields include:

- `sku_id`
- `forecast_target`
- `forecast_conservative`
- `forecast_aggressive`
- `aggressive_forecast_weekly_avg`
- `past_6_month_weekly_moving_avg`
- seasonal weekly forecast columns
- confidence and review flags

## Portfolio note

This repository is meant to show the structure and modeling logic of a supply-chain analytics project. It should not be treated as a direct copy of any employer-owned production system.
