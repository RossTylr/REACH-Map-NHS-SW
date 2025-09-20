# Baseline coverage build

_Run: 2025-09-20 17:09_

## Data & config

- LSOAs: **336**  | Stations: **14**  | Acutes: **3**
- Response thresholds: [7, 15, 18, 40] minutes
- Conveyance thresholds: [30, 45, 60] minutes
- Blue-light factors: response=1.0, convey=1.0
- MAX_RADIUS_MIN: None
- Cleaning: ALLOW_DIAGONAL_ZERO=True, FLOOR_OFFDIAG_ZERO_MIN=0.5, DROP_NEGATIVES=True, MAX_SANITY_MIN=300

## Matrices

- R (station→LSOA): shape (336, 14), nnz 4,690, density 0.997024
- C (LSOA→acute): shape (336, 3), nnz 1,005, density 0.997024
- Files: R=0.02 MB, C=0.00 MB

## Quantiles (minutes)

- Response: min 0.5, p25 5.7, median 11.22, p90 24.64, p95 28.76, max 92.22 (n=336)
- Conveyance: min 0.5, p25 16.95, median 29.28, p90 74.47, p95 80.82, max 129.12 (n=336)

## Coverage (% population ≤ band)

    metric  threshold_min  pop_covered_pct  population_total
conveyance             30        51.771427            575628
conveyance             45        69.350193            575628
conveyance             60        79.026059            575628
  response              7        30.774565            575628
  response             15        62.333893            575628
  response             18        75.153048            575628
  response             40        99.262392            575628