# VitalCare Wellness Group: Patient Cost and Outcomes Analysis

A data cleaning and dashboard project built in Google Sheets on a synthetic health dataset. It cleans 1,525 raw visit records, answers four business questions with pivot tables, and presents the results in an interactive dashboard.

> **Note:** all data is synthetic and VitalCare Wellness Group is a fictional clinic chain. This project was created for portfolio use.

![Dashboard preview](images/dashboard.png)

## Business problem

VitalCare operates clinics in five regions of Ghana: Accra, Kumasi, Takoradi, Tamale and Cape Coast. Management wants to know where costs and outcomes differ, so it can plan pricing, staffing and care programmes. The dataset covers patient visits from January 2023 to December 2024.

## Questions answered

1. What is the average cost per visit by region and insurance status?
2. When do the main conditions peak during the year?
3. How do treatment outcomes differ by insurance status?
4. How do treatment outcomes differ by condition?

## Key findings

| Question | Finding |
| --- | --- |
| Q1. Cost | Uninsured patients pay more in every region. Tamale (GHS 267.91) and Cape Coast (GHS 228.29) are the most expensive. The overall average is GHS 198.73. |
| Q2. Seasonality | Typhoid peaks Mar to May, malaria Apr to Jul, and respiratory infection Jun to Aug. |
| Q3. Outcome by insurance | Recovery: Insured 88.60%, Uninsured 85.85%, Unknown 83.33%. Uninsured patients are referred to specialists more often (4.52% vs 2.74%). |
| Q4. Outcome by condition | Maternal Checkup has the highest recovery rate (95.22%). Diabetes has the lowest (72.66%). |

Even insured patients in Tamale (GHS 243.31) pay more than uninsured patients in Accra (GHS 216.00), so Tamale and Cape Coast have a higher base cost whatever the insurance status.

## Data cleaning summary

The raw file had 1,525 rows. After cleaning, 1,457 remain.

| Step | Issue | Action | Result |
| --- | --- | --- | --- |
| 1 | No untouched original | Duplicated the raw data tab before any edits | Raw Data tab preserved |
| 2 | Loose range with no structure | Converted the working range to a table (`tblHealthClean`) | Filters and structured references |
| 3 | Mixed DD/MM and MM/DD dates | Standardized `VisitDate` to one format and documented the assumption for ambiguous dates | Consistent dates |
| 4 | Duplicate visits | Matched PatientID + VisitDate + Condition, kept the most complete row | 1,525 to 1,457 rows |
| 5 | PatientID reused for different patients | Assigned the correct ID to each patient name | Unique patient IDs |
| 6 | Inconsistent Region and Gender labels | Trimmed spaces and normalized casing | One label per category |
| 7 | InsuranceStatus as Yes/No or Insured/Uninsured | Unified into two values | Insured / Uninsured |
| 8 | Invalid ages (for example 187) | Filled from the same patient's other visits, otherwise left blank | 32 blanks remain |
| 9 | Negative treatment costs | Converted with `ABS()` | No negatives |
| 10 | Extreme cost outlier (V101230) | Replaced with the condition median (GHS 83.92) | No outliers |
| 11 | Blank InsuranceStatus | Labeled "Unknown" | 36 rows |
| 12 | Blank Outcome | Labeled "Not Recorded" | 77 rows |
| 13 | Blank treatment costs | Filled with the median for the same condition | No blanks |
| 14 | No month field for seasonality | Added a `VisitMonth` helper column | Months sort correctly |

Two decisions worth noting:

- Ages were left blank rather than guessed, because a wrong age is worse than a missing one.
- Costs were filled with the median for the same condition, not the overall median, because treatment costs vary widely between conditions.

## Dashboard

The dashboard combines four charts, three slicers (Condition, InsuranceStatus, Region) and a one-line takeaway under each chart.

- Q1: clustered column chart of average cost by region and insurance status
- Q2: line chart of monthly visits by condition, with the three seasonal conditions highlighted
- Q3: 100% stacked bar of outcomes by insurance status
- Q4: 100% stacked bar of outcomes by condition

## Recommendations

1. Review pricing and insurance enrolment in Tamale and Cape Coast.
2. Plan staffing and stock around the seasonal peaks.
3. Set up a structured follow-up programme for diabetes.
4. Improve outcome recording, since "Not Recorded" appears in every group.

These are suggestions from patterns in synthetic data and would need testing against real clinic data.

## Limitations

- The data is synthetic, so the findings demonstrate a method and not real clinic performance.
- The analysis shows patterns only, not causes.
- 32 patient ages are blank, so no age-based analysis was done.
- Visit months combine 2023 and 2024, which shows seasonality but hides year-on-year change.

## Tools and skills

- Google Sheets: formulas (`COUNTIFS`, `TEXT`, `ABS`), tables, pivot tables, charts, slicers
- Data cleaning: date standardization, duplicate detection, group-based median imputation, label standardization
- Dashboard design: layout, colour palette, takeaway text
- Documentation: data dictionary, cleaning log, case study

## Repository structure

```
vitalcare-health-analysis/
├── README.md
├── data/
│   ├── VitalCare_Health_Dataset_raw.xlsx      # original synthetic data
│   └── VitalCare_Health_Dataset_final.xlsx    # cleaned data, pivots and dashboard
├── docs/
│   ├── case_study.pdf
│   └── data_cleaning_process.docx
└── images/
    └── dashboard.png
```

## Next steps

- Extend the analysis with the sales and combined datasets.
- Test cost differences by condition mix within each region.

## Author

Tricky, Data Analyst, Accra, Ghana
