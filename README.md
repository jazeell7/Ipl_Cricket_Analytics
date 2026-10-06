# IPL Match Data Preparation

## Project Overview

This project cleans and prepares an IPL cricket match dataset using SQL
with SQLite. A Python notebook is used to explore the cleaned data.

The original CSV is preserved in `data/raw/`.

## Dataset

- **Dataset:** IPL cricket match data
- **Original file:** `data/raw/matches.csv`
- **Total records:** 1,212 matches
- **Original columns:** 28

## Project Structure

```text
data-preparation-assignment/
├── README.md
├── data/
│   ├── raw/
│   │   └── matches.csv
│   └── processed/
├── sql/
│   ├── 01_nulls_and_blanks.sql
│   ├── 02_merge_categories.sql
│   ├── 03_venue_names.sql
│   ├── 04_dedupe_venues.sql
│   ├── 05_city_fallback.sql
│   ├── 06_season_year.sql
│   ├── 07_win_definition.sql
│   └── 08_matches_clean.sql
├── notebooks/
│   └── data_exploration.ipynb
└── output/
    └── matches_clean.csv
```

## Data Cleaning Steps

1. **Nulls and blanks:** Checked missing and blank values and created
   a working table for cleaning.
2. **Category standardization:** Trimmed whitespace from team and
   toss-winner category values while preserving historical team names.
3. **Venue standardization:** Standardized known venue-name variants.
4. **Venue deduplication:** Combined additional clear venue variants.
5. **City fallback:** Filled missing city values using known venues.
   Dubai International Cricket Stadium was mapped to Dubai, and
   Sharjah Cricket Stadium was mapped to Sharjah.
6. **Season year:** Added a numeric `season_year` based on the first
   four characters of the season label. The original label is preserved.
7. **Win definition:** Added `team1_won`: 1 if Team 1 won, 0 if Team 2
   won, and NULL when no result or the winner could not be determined.
8. **Clean output:** Created the final cleaned table and exported it
   as `output/matches_clean.csv`.

## Data Quality Notes

- The raw dataset contains 1,212 records.
- Initially, 51 city values were missing; these were filled using venue
  information.
- 9 matches had no player-of-the-match value.
- No duplicate complete rows or duplicate match IDs were found during
  the initial inspection.
- Win-by-runs and win-by-wickets values can be NULL when that result
  type does not apply; they were not automatically replaced with zero.
- Historical team names were retained rather than merged into a single
  modern franchise name.

## Final Output

The cleaned CSV contains **1,212 rows and 32 columns**.

The `team1_won` column contains:
- `1`: 596 matches
- `0`: 607 matches
- `NULL`: 9 matches

## Tools Used

- Python
- SQLite (`sqlite3`)
- pandas
- Jupyter Notebook
- Git and GitHub

## How to Run

1. Clone or download this repository.
2. Ensure Python and pandas are installed.
3. From the project root, create the SQLite database and import
   `data/raw/matches.csv` into a table named `matches_raw`.
4. Execute the SQL scripts in numerical order, from `01` to `08`,
   against the same database.
5. Export the `matches_clean` table to `output/matches_clean.csv`.
6. Open `notebooks/data_exploration.ipynb` in Jupyter to explore
   the cleaned dataset.

The SQL scripts are kept separately so the cleaning steps can be
reviewed and reproduced.
