# Data Cleaning Log — COVID-19 Cases & Vaccination Analysis

## 1. Dataset Overview

The project uses three original CSV datasets:

| Dataset | Description |
|---|---|
| `case_time_series - case_time_series.csv` | Daily COVID-19 case trends in India. |
| `raw_data_indian_states.csv` | India and state-level COVID-19 information. |
| `vaccination_data.csv` | Country-level vaccination information. |

- **COVID-19 case data period:** January 30, 2020 – October 31, 2021
- **Tool:** Power BI Desktop
- **Data preparation:** Power Query
- **Duplicate records:** No duplicates found during the cleaning checks.

## 2. Cleaning Steps

| Issue | Action Taken |
|---|---|
| Date fields had different formats or data types. | Converted date fields to the Date data type. |
| Case trend column names needed standardization. | Renamed columns for easier understanding and analysis. |
| Vaccine names contained unnecessary spaces. | Applied the Trim transformation to remove extra spaces. |
| Multiple vaccine types were stored together in `VACCINES_USED`. | Created a separate `Vaccine_Types` Reference query and split the vaccine names by comma into separate rows. |
| Missing values in the `Tested` column. | Preserved missing values rather than replacing them with zero. |
| Blank values in some vaccination columns. | Preserved blank values rather than assuming they represented zero. |
| Duplicate records. | Checked the datasets; no duplicates were found during the checks. |

## 3. Handling Missing Values

### Tested Column

Approximately **69% of the values** in the `Tested` column of the state-level dataset were missing.

These values were preserved rather than replaced with zero because a missing value does not necessarily mean that no tests were conducted.

### Vaccination Data

Some vaccination columns contained blank values.

The blanks were preserved instead of being replaced with zero to avoid treating unavailable information as a confirmed zero value.

## 4. Data Transformation

The following transformations were completed using Power Query:

1. Converted date columns to the Date data type.
2. Renamed case trend columns for easier analysis.
3. Removed unnecessary spaces from vaccine names using Trim.
4. Created a Reference query named `Vaccine_Types` from the original `vaccination-data` table.
5. Split the `VACCINES_USED` column by comma into separate rows.
6. Retained the country and vaccine name columns in `Vaccine_Types`.
7. Preserved missing testing and vaccination values.
8. Checked for duplicate records.

Repeated country names in `Vaccine_Types` are expected because a country can have multiple recorded vaccine types.

The vaccine-type chart counts country-vaccine entries. For example, Pfizer-BioNTech – Comirnaty recorded **163 entries (19.06%)** in the chart. This represents vaccine-type entries, not doses administered.

## 5. Data Modeling

Created the following relationships in Power BI:

**Relationship 1: COVID-19 Case Data**

- `case_time_series - case_time_series[Date_YMD]` → `raw_data_indian_states[Date]`
- One-to-many relationship using the date fields.

**Relationship 2: Vaccination Data**

- `vaccination-data[COUNTRY]` → `Vaccine_Types[COUNTRY]`
- One-to-many relationship using the country field.

These relationships organize the datasets for dashboard analysis.

## 6. Final Data Check

- COVID-19 case data period: **January 30, 2020 – October 31, 2021**
- Duplicate records: **0 found during checks**
- Missing values in `Tested`: Preserved.
- Blank vaccination values: Preserved.
- Date fields: Converted to Date data type.
- Vaccine names: Trimmed and split into separate rows in `Vaccine_Types`.
- Data relationships: Both relationships created successfully.
- Vaccine-type entries: Counted separately from vaccination doses.

## 7. Output

The transformed tables were loaded into the Power BI data model and used to create the interactive dashboard.

The final Power BI dashboard is available as:

`PowerBI/COVID-19_Dashboard.pbix`