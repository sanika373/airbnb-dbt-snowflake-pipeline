# Airbnb End-to-End Data Engineering Pipeline
### dbt Core · Snowflake · AWS S3 · Medallion Architecture

---

## Overview

End-to-end ELT pipeline ingesting Airbnb listings, reviews, and host data from **AWS S3** into **Snowflake** using a **Medallion Architecture** (Bronze, Silver, Gold). Built with **dbt Core** for all transformation layers, with full data lineage, automated data observability checks, and enforced data contracts across every layer.

---

## Architecture

```
AWS S3 (Raw Data)
      │
      ▼
 Snowflake (Bronze) ──► Python External Stage Ingestion
      │
      ▼
 Staging Layer       ──► dbt Core source freshness + not_null checks
      │
      ▼
 Intermediate Layer  ──► dbt Core CTEs, deduplication, type casting
      │
      ▼
 Mart / OBT Layer    ──► dbt Core final models, SCD Type 2 snapshots
```

---

## Tech Stack

| Layer | Tool |
|---|---|
| Cloud Storage | AWS S3 |
| Data Warehouse | Snowflake |
| Transformation | dbt Core |
| Ingestion | Python (External Stage) |
| Data Modeling | Medallion Architecture (Bronze / Silver / Gold) |
| Historization | SCD Type 2 Snapshots |
| Data Quality | dbt tests (not_null, unique, accepted_values, relationships, source freshness) |

---

## Key Features

- **Medallion Architecture** — Bronze (raw), Silver (cleaned), Gold (mart/OBT) layers with clear separation of concerns
- **SCD Type 2 Snapshots** — tracks host and listing history over time, preserving point-in-time accuracy
- **Reusable dbt Macros** — custom macros for deduplication, type casting, and surrogate key generation
- **Data Observability** — automated tests enforcing not_null, unique, accepted_values, referential integrity, and source freshness across all layers
- **Data Contracts** — schema-level contracts enforced at every transformation boundary to prevent drift
- **Full Data Lineage** — end-to-end lineage visible in dbt docs from S3 source to final mart models

---

## Project Structure

```
airbnb-dbt-snowflake-pipeline/
├── DDL/                  # Snowflake table definitions and external stage setup
├── models/
│   ├── staging/          # Raw source models, type casting, renaming
│   ├── intermediate/     # Business logic, joins, deduplication
│   └── marts/            # Final OBT and dimensional models
├── snapshots/            # SCD Type 2 host and listing history
├── macros/               # Reusable dbt macros
├── tests/                # Custom data quality tests
└── dbt_project.yml
```

---

## Data Sources

| Dataset | Description |
|---|---|
| Listings | Airbnb property details, pricing, availability |
| Reviews | Guest reviews with sentiment and date |
| Hosts | Host profile and verification data |

---

## Data Quality Checks

All models include dbt native tests:
- `not_null` — critical ID and date fields
- `unique` — surrogate and natural keys
- `accepted_values` — categorical fields (room type, property type)
- `relationships` — referential integrity across fact and dimension models
- `source freshness` — alerts when source data exceeds expected latency

---

## How to Run

```bash
# Install dbt
pip install dbt-snowflake

# Set up your Snowflake profile in ~/.dbt/profiles.yml

# Install dependencies
dbt deps

# Run all models
dbt run

# Run data quality tests
dbt test

# Generate and serve documentation
dbt docs generate
dbt docs serve
```

---

## Author

**Sanika Pawar** — Data Engineer / Analytics Engineer  
[linkedin.com/in/sanika-pawar7481](https://linkedin.com/in/sanika-pawar7481) · [github.com/sanika373](https://github.com/sanika373)
