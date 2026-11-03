# Hotel Booking Analysis

## Description
Cleaning and analysis of 10,325 deliberately corrupted booking records from two Portuguese hotels. The pipeline audits the data, cleans it rule by rule (with a log and row-level lineage), and answers ten descriptive questions on cancellation, seasonality, lead time, guest origin, market segments, deposits, price and length of stay.

**Key figures:** 10,325 rows → 10,000 bookings (unit of analysis = one `booking_id`); overall cancellation rate 37.41%; City Hotel 41.79% versus Resort Hotel 28.50%. Findings are descriptive and carry no causal interpretation.

## Structure

``` text
hotel-booking-analysis/
├── README.md
├── hotel_analysis.ipynb          Full pipeline (Restart & Run All)
├── Bao_cao_Khach_san.pdf         Report
├── data/hotel_bookings_dirty.csv Raw input, never modified
└── outputs/
    ├── bookings_clean.csv  monthly_summary.csv  cleaning_log.csv
    ├── row_lineage.csv     quality_report.csv
    ├── tables/             14 source tables
    └── figures/            8 figures
```

# Language

**Main Language:** python

**Library:** `pandas`, `numpy`, `matplotlib`.

## Project-specific rules

| Topic | Rule |
|---|---|
| Duplicates | Normalise `booking_id` (trim, upper-case); drop exact duplicates; merge rows with the same ID taking the first non-missing value by ascending `row_id`. Conflicts (15 bookings) are recorded in `row_lineage.note` |
| Cancellation flag | `reservation_status` prevails over `is_canceled` when valid (40 rows corrected) |
| Missing values | Kept as missing: no imputation, no row deletion. `meal` `Undefined` → `SC`; `Undefined` in `market_segment` and `distribution_channel` → missing |
| Dates | Four formats read explicitly (`dd/mm/yyyy` day first). Valid ranges: arrival 2015-07-01 to 2017-08-31; status update 2014-10-01 to 2017-09-14 |
| Numeric domains | `lead_time` 0–800; weekend nights 0–20; week nights 0–50; guests 0–10; special requests 0–5; ADR 0–1,000. Outside the range → missing. ADR = 0, zero-night and zero-guest bookings are retained |
| Country | Upper-case ISO codes; `UK` and five full names mapped; `CN` kept distinct from `CHN` |
| Denominators | Cancellation rate = `is_canceled` = 1 ÷ non-missing `is_canceled`. ADR and stay analyses use non-cancelled bookings only (ADR > 0 for price) |

## Data source and licence

Antonio, N., de Almeida, A., & Nunes, L. (2019). *Hotel booking demand datasets.* Data in Brief, 22, 41–49. <https://doi.org/10.1016/j.dib.2018.11.126> (CC BY 4.0). Educational adaptation: a random sample of the original 119,390 bookings with errors and duplicates inserted deliberately; accessed 4 October 2026.

**Limitations:** two hotels in Portugal only; 2016 is the only complete year (arrivals 2015-07 to 2017-08); no realised revenue, cancellation reasons or guest reviews (`adr` is a price per night, not revenue); `country` may be less reliable for cancelled bookings.

## Team

| # | Member | Student ID | Responsibility | GitHub |
|---|---|---|---| --- |
| 1 | `Hà Nhật Huy` | `11253519` |  |
| 2 | `Lê Minh Quân` | `11257109` |  |


Group `NhomXX` · last full re-run `DD/MM/YYYY` · 

AI tools used: `[tool and purpose, or “None”]`. 

Every member can explain the code and figures in their part.
