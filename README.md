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

Open `parking.ipynb` in Jupyter and run all cells in order.

## Project Structure

```
├── parking.ipynb                          # Main analysis notebook
├── Annual_Parking_Study_Data_20251203.csv # Historical parking study data (2014–2019)
└── README.md
```

## Key Findings

- **High-demand areas**: First Hill, Green Lake, Cherry Hill, Westlake, Uptown, Fremont — consistently high occupancy, some with prices that could be revised upward.
- **Low-demand areas**: Roosevelt, Columbia City, Commercial Core — persistently low occupancy suggesting opportunities to reduce prices or repurpose spaces.
- Construction and event closures both suppress occupancy significantly (from ~0.77 to ~0.31).
- Side of street has no meaningful effect on utilisation.

---

## Business Recommendations

1. **Raise rates where occupancy is persistently high.** First Hill, Green Lake, Cherry Hill,
   Westlake, Uptown and Fremont run consistently full — a signal that current pricing sits below
   what demand supports, and that drivers are circling for spaces.
2. **Reduce rates or repurpose kerb space where occupancy stays low.** Roosevelt, Columbia City
   and the Commercial Core show persistent under-use; price cuts, or conversion to loading,
   bike or transit space, are both worth testing.
3. **Treat construction and event closures as a planned capacity loss, not noise.** Occupancy
   falls from roughly 0.77 to 0.31 during closures — large enough that it should be modelled
   explicitly in any utilisation target.
4. **Stop differentiating by side of street.** It has no meaningful effect on utilisation, so it
   is not worth carrying as a pricing or planning variable.

---

## Limitations

- The annual study data covers **2014–2019**; the transaction data is a **single week** in
  December 2025. The two sources are not directly comparable, and neither reflects current
  post-pandemic commuting patterns.
- The parking study is a **periodic manual survey**, not continuous sensor data, so occupancy is
  a sampled snapshot rather than a true average.
- One week of transaction data cannot separate seasonal effects from underlying demand — early
  December is atypical.
- The analysis is **descriptive**: it identifies where price and demand appear misaligned, but
  does not estimate a demand elasticity, so the size of any rate change is not derived from the
  data.

---

## Future Improvements

- Estimate price elasticity per study area to size rate changes rather than only direction
- Extend the transaction data to a full year to separate seasonality from demand
- Join in transit and construction-permit data to control for supply-side shocks
