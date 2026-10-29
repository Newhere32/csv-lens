# CSV Lens

A dependency-free CSV profiler for data engineers who need a fast first pass before analysis or import. It reports schema shape, inferred types, missingness, bounded distinct counts, malformed row widths, duplicate rows, duplicate headers, and sensitivity hints based on column names.

## Why it exists

CSV files often look plausible while containing ambiguous types, inconsistent rows, hidden missing values, or repeated records. CSV Lens emits a reproducible report without copying raw cell values into it, making it safer to attach summaries to issue tickets.

## Features

- One-pass profiling using Python's CSV parser (handles quoted commas and escaped quotes).
- UTF-8 and UTF-8-with-BOM input support.
- Type inference for booleans, integers, numbers, dates, datetimes, strings, mixed, and unknown columns.
- Missingness and distinct counts with a configurable per-column memory cap.
- Exact duplicate-row count using row hashes; no cell values are emitted.
- Header/row-width diagnostics and heuristics for potentially sensitive column names.
- Text output for humans and JSON output for automation.

## Install and use

Requires Python 3.10+; no runtime dependencies.

```bash
python -m pip install -e .
csv-lens data/customers.csv
csv-lens data/events.csv --format json --output events-profile.json
csv-lens data.tsv --delimiter $'\t' --max-distinct 20000
```

On Windows PowerShell, pass a literal tab using `--delimiter "`t"`.

Exit codes: `0` means the file parsed and no structural header/row-width issue was found; `1` means structural issues were detected; `2` indicates invalid input or an operational error. Duplicate rows are reported but do not alone change the exit status.

## Reading the report

- `distinct_count_is_lower_bound: true` means the per-column cap was reached; the value is a lower bound, not an exact cardinality.
- `inferred_type` is heuristic, not a schema contract. Identifiers that contain only digits may be inferred as numeric; keep domain semantics in mind.
- `sensitivity_hints` rely on header names only and do not inspect values to identify PII.
- Blank values and the strings `NA`, `N/A`, `null`, `none`, and `nan` are considered missing, case-insensitively.

## Memory characteristics and limitations

Per-column distinct tracking is capped by `--max-distinct`. Exact duplicate-row counting currently retains one SHA-256 digest per distinct row, so memory for duplicate detection grows with the number of distinct rows. The tool does not mutate the input file and does not include example values in its report.

## Development

```bash
python -m pip install -e .
python -m compileall -q src tests
python -m unittest discover -s tests -v
```

See [CONTRIBUTING.md](CONTRIBUTING.md) and [SECURITY.md](SECURITY.md).
## Project status

This is an AI-assisted initial implementation released as an alpha. Validate its profiling assumptions against your actual data conventions before using the report as a schema contract. It does not claim to identify all sensitive data.
