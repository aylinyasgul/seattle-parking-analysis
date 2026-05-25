# Parking Analysis in Seattle

Seattle ranks among the top 10 most congested cities in the United States. This project analyses parking utilisation and pricing across Seattle to help the Seattle Department of Transportation (SDOT) optimise parking spaces.

## Business Questions

1. How does utilisation vary across Seattle?
2. How has utilisation varied across the years?
3. Are prices aligned with demand in different study areas?

## Datasets

| Dataset | Format | Coverage |
|---|---|---|
| [Annual Parking Study Data](https://data.seattle.gov/Transportation/Annual-Parking-Study-Data/7jzm-ucez) | CSV | 2014–2019 |
| [Paid Parking Transaction Data](https://data.seattle.gov/Transportation/Paid-Parking-Transaction-Data/gg89-k5p6/about_data) | JSON | 29 Nov – 5 Dec 2025 |

Both datasets are publicly available from the City of Seattle Open Data Portal. The CSV file (`Annual_Parking_Study_Data_20251203.csv`) is included in this repository.

## Tech Stack

- **Apache Spark (PySpark)** — distributed data processing
- **MinIO** — local S3-compatible object storage for loading datasets into Spark
- **pandas / matplotlib / seaborn** — analysis and visualisation

## Setup

### 1. Start MinIO locally

Run a MinIO instance on `http://localhost:9000` and upload both datasets to a bucket named `parking`:

```
parking/
  csv/   ← Annual Parking Study Data CSV
  json/  ← Paid Parking Transaction Data JSON
```

### 2. Set environment variables

```bash
export MINIO_ACCESS_KEY=your_access_key
export MINIO_SECRET_KEY=your_secret_key
```

### 3. Run the notebook

Open `parking.ipynb` in Jupyter and run all cells, or execute `parking.py` directly:

```bash
python parking.py
```

## Project Structure

```
├── parking.ipynb                          # Main analysis notebook
├── parking.py                             # Notebook exported as a Python script
├── Annual_Parking_Study_Data_20251203.csv # Historical parking study data (2014–2019)
└── README.md
```

## Key Findings

- **High-demand areas**: First Hill, Green Lake, Cherry Hill, Westlake, Uptown, Fremont — consistently high occupancy, some with prices that could be revised upward.
- **Low-demand areas**: Roosevelt, Columbia City, Commercial Core — persistently low occupancy suggesting opportunities to reduce prices or repurpose spaces.
- Construction and event closures both suppress occupancy significantly (from ~0.77 to ~0.31).
- Side of street has no meaningful effect on utilisation.
