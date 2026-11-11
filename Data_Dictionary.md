# Data Dictionary — COVID-19 Cases & Vaccination Analysis

## Project Overview

The project uses three original CSV datasets:

- `case_time_series - case_time_series.csv`
- `raw_data_indian_states.csv`
- `vaccination_data.csv`

These datasets contain daily COVID-19 case trends, Indian state-level COVID-19 information, and global vaccination records.

A separate `Vaccine_Types` table was created in Power Query from the original vaccination dataset for vaccine-type analysis.

## 1. case_time_series - case_time_series

This table contains daily COVID-19 case information for India.

| Column | Description |
|---|---|
| `Date` | Date of the COVID-19 record. |
| `Date_YMD` | Date represented in year-month-day format for analysis. |
| `Daily Confirmed` | Number of newly confirmed COVID-19 cases. |
| `Total Confirmed` | Cumulative number of confirmed COVID-19 cases. |
| `Daily Recovered` | Number of newly recovered cases. |
| `Total Recovered` | Cumulative number of recovered COVID-19 cases. |
| `Daily Deceased` | Number of newly deceased cases. |
| `Total Deceased` | Cumulative number of deceased COVID-19 cases. |

### Purpose

This table is used for:

- Case Trend visualization.
- Date-based analysis.
- Comparing daily confirmed cases, recoveries, and deaths over time.

## 2. raw_data_indian_states

This table contains COVID-19 statistics for India and individual Indian states.

| Column | Description |
|---|---|
| `Date` | Date of the COVID-19 record. |
| `State` | India or an individual Indian state/region. |
| `Confirmed` | Cumulative number of confirmed COVID-19 cases. |
| `Recovered` | Cumulative number of recovered COVID-19 cases. |
| `Deceased` | Cumulative number of deceased COVID-19 cases. |
| `Tested` | Number of COVID-19 tests reported, where available. |
| Other source columns | Additional state-level information available in the original dataset. |

### National KPI Calculation

The `State` column contains both an India aggregate row and individual Indian state records.

The national KPI calculations use:

`State = India`

This ensures that national totals are taken from the India aggregate row rather than adding individual state records to the national total.

### Purpose

This table is used for:

- Total Confirmed Cases.
- Total Recoveries.
- Total Deaths.
- Cases by State visualization.
- State slicer.

## 3. vaccination-data

This table contains country-level COVID-19 vaccination information.

The source file is `vaccination_data.csv`. The table name used in Power BI is `vaccination-data`.

| Column | Description |
|---|---|
| `COUNTRY` | Country name. |
| `ISO3` | Three-letter country code. |
| `WHO_REGION` | WHO region associated with the country. |
| `DATA_SOURCE` | Source of the reported vaccination information. |
| `DATE_UPDATED` | Date when the vaccination information was last updated. |
| `FIRST_VACCINE_DATE` | Date when the first vaccination was administered in the country. |
| `TOTAL_VACCINATIONS` | Total number of vaccination doses recorded. |
| `PERSONS_VACCINATED_1PLUS_DOSE` | Number of people receiving at least one vaccine dose. |
| `TOTAL_VACCINATIONS_PER100` | Total vaccination doses per 100 population. |
| `PERSONS_VACCINATED_1PLUS_DOSE_PER100` | People receiving at least one dose per 100 population. |
| `PERSONS_FULLY_VACCINATED` | Number of fully vaccinated people. |
| `PERSONS_FULLY_VACCINATED_PER100` | Fully vaccinated people per 100 population. |
| `VACCINES_USED` | Vaccine types reported for the country. |

### Purpose

This table is used for:

- Total Vaccinations KPI.
- Average Fully Vaccinated KPI.
- Top 10 Vaccinations visualization.
- Vaccination Progress (%) gauge.
- Country slicer.
- Region slicer.

## 4. Vaccine_Types

This is a separate Reference query created from the original `vaccination-data` table using Power Query.

The `VACCINES_USED` column was split by comma into separate rows. Extra spaces were trimmed, and blank entries were removed.

| Column | Description |
|---|---|
| `COUNTRY` | Country associated with the recorded vaccine type. |
| `VACCINES_USED` | Individual vaccine type recorded for that country. |

Repeated country names are expected because one country can have multiple recorded vaccine types.

### Purpose

This table is used for:

- Vaccine Types donut chart.
- Comparing the vaccine types recorded across countries.

The chart counts country-vaccine entries, not vaccination doses.

## 5. Data Transformation

The following transformations were performed using Power Query:

- Converted date fields to the Date data type.
- Renamed case trend columns for easier analysis.
- Trimmed vaccine names to remove unnecessary spaces.
- Created a Reference query named `Vaccine_Types` from `vaccination-data`.
- Split `VACCINES_USED` by comma into separate rows.
- Retained the country and vaccine name columns in `Vaccine_Types`.
- Preserved blank vaccination and testing values.
- Checked the datasets for duplicate records.

## 6. Data Model

Two one-to-many relationships were created in Power BI.

**Relationship 1: COVID-19 Case Data**

- `case_time_series - case_time_series[Date_YMD]`
- `raw_data_indian_states[Date]`

Relationship: **1 → Many**

**Relationship 2: Vaccination Data**

- `vaccination-data[COUNTRY]`
- `Vaccine_Types[COUNTRY]`

Relationship: **1 → Many**

The original vaccination table is used for country-level vaccination calculations, while `Vaccine_Types` is used for individual vaccine-type analysis.

## 7. Important Calculation Notes

- The first three national KPIs represent India's cumulative totals on the latest available date in the state-level dataset.
- Total Confirmed Cases (India): approximately **34.3 million**.
- Total Recoveries (India): approximately **33.7 million**.
- Total Deaths (India): approximately **458 thousand**.
- Total Vaccinations represents the sum of recorded vaccination doses across 228 countries, approximately **8.2 billion**.
- Average Fully Vaccinated is **43.57%**, calculated as the arithmetic average of available country-level fully vaccinated percentages.
- The fully vaccinated percentage is not a population-weighted global vaccination rate.
- Maharashtra recorded **6,611,078 confirmed cases** and **6,450,585 recoveries** in the latest state-wise figures.
- China ranked first in the Top 10 Vaccinations chart, with **2,543,206,400 recorded vaccination doses**.
- Pfizer-BioNTech – Comirnaty recorded **163 country-vaccine entries (19.06%)** in the Vaccine Types chart.
- Vaccine Types chart counts represent country-vaccine entries, not doses administered.
- The May 2021 case trend recorded **9,016,687 confirmed cases, 10,266,501 recoveries, and 120,072 deaths** as monthly totals of daily reported values.