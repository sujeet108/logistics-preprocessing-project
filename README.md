# Logistics Data Collection, Cleaning and Preprocessing Pipeline

Week 2 task: a reproducible Python pipeline that prepares shipment data for logistics analytics.

## What this project does
1. **Data collection (simulated):** `src/generate_dataset.py` builds a raw shipment dataset (4,732 rows, 17 columns)
   modelled on public supply-chain datasets (e.g. Kaggle *DataCo Smart Supply Chain*). Realistic quality problems
   are injected on purpose: missing values, duplicates, inconsistent labels, invalid dates, outliers, currency text.
2. **Cleaning and preprocessing:** `src/preprocessing_pipeline.py`
   - standardises column names and category labels
   - fixes data types (dates, `"$1,234.50"` strings)
   - removes duplicates
   - flags and repairs invalid values
   - imputes missing values (group-wise median / explicit "Unknown")
   - detects outliers (IQR, log-scale IQR, Z-score) and winsorises them
   - engineers features (delay days, late flag, cost per km / kg)
   - scales (Min-Max, Z-score), log-transforms skewed columns and one-hot encodes categories
3. **Outputs:** cleaned data, model-ready data, charts and a `metrics.json` summary.

## Project structure
```
data/        raw_logistics_shipments.csv, cleaned_logistics_shipments.csv, model_ready_logistics_shipments.csv
src/         generate_dataset.py, preprocessing_pipeline.py
outputs/     metrics.json, figures/*.png
report/      Logistics_Data_Preprocessing_Report.docx
```

## How to run
```bash
pip install -r requirements.txt
python src/generate_dataset.py          # creates data/raw_logistics_shipments.csv
python src/preprocessing_pipeline.py    # creates cleaned + model-ready files, figures, metrics
```

## Key results
| Metric | Raw | Cleaned |
|---|---|---|
| Rows | 4,732 | 4,594 (2.9% removed) |
| Duplicate rows | 138 | 0 |
| Missing cells | 1,688 | 36 (unfillable delivery dates, kept as NaT) |
| Transport-mode labels | 13 | 4 |
| Weight std. dev. (kg) | 2,001.5 | 116.5 |

## Note on data
The dataset is **synthetic** (seeded with `numpy.random.default_rng(42)`, so results are reproducible). Its schema follows
public logistics datasets, but its numbers are not real shipments, so business findings (e.g. late-delivery rates)
illustrate the method and should not be read as real-world results.

## Author
Sujeet Kumar
