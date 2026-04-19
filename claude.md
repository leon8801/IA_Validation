# IA_Validation

Validates fixed indexed annuity policy values by comparing calculated results
against Oracle SQL system values.

## Project Structure

- `queries/`           - Pre-formed SQL query scripts (named by cycle date, e.g. `cycle_2026_03.sql`)
- `scripts/`           - Utility scripts (DB connection, validation runner, export, etc.)
- `config/`             - DB configuration file (credentials here, NOT in env vars)
- `requirements.txt`
- Flat layout — no src/ or tests/ directories

## Key Concepts

- **Transaction types**: affect interest credited calculations
- **Equity index levels, strategy rates**: caps, participation rates, spreads
- **Credited interest**: calculated per policy/strategy based on index performance
- **Policy values**: AV (Account Value), CSV (Cash Surrender Value), DB (Death Benefit),
  MWV (Market Value), surrender charge

## DB Connection

- Oracle SQL via cx_Oracle
- Config file: `config/db.json` (or `config.ini`)
  ```json
  { "user": "...", "password": "...", "dsn": "..." }
  ```
- Connection utility in `scripts/db_connect.py`

## Comparison Tolerance

- Dollar amounts (AV, CSV, etc.): 4 decimal places
- Percentages (rates, caps, spreads): 2 decimal places

## Common Commands

```bash
pip install -r requirements.txt
python scripts/run_validation.py --cycle YYYY_MM
python scripts/run_validation.py --policy POL-12345
python scripts/export_results.py --format csv
```

## Query Script Naming

- By cycle date: `queries/cycle_YYYY_MM.sql`
- Returns transaction-level data for all policies in that cycle

## Output

- CSV format for validation results
- Discrepancy report: policy, field, expected, actual, variance
