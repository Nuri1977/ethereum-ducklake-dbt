# Ethereum data lake with DuckLake

A data lake on your laptop: three months of public Ethereum data in [DuckLake](https://ducklake.select/), with dbt models on top. Built step by step in [Rosetta DBT Studio](https://rosettadb.io/download-dbtstudio.html), all open source.

Code for the free course **Data Lakes with DuckLake in Rosetta DBT Studio**. One commit per lesson, so you can compare your project with this one at any step.

- Full course video (~70 min): https://youtu.be/-MQ_tMo3lU0
- Rosetta Academy: https://academy.rosettadb.io/

## What is in it

| What | Where |
|---|---|
| Lake | DuckLake with a DuckDB catalog: `lake/eth_catalog.duckdb` + Parquet files in `lake/data/` (not in git) |
| Raw data | `raw.blocks` (Aug to Oct 2026, 659,896 blocks), `raw.transactions` (1 to 7 Oct 2026, 12.7M rows, partitioned by day) |
| SQL per lesson | `sql/`: load, snapshots, time travel, update and delete, daily loads, schema evolution, change feed, partitioning, statistics, inlining, maintenance, adding files, catalog tables, reading the lake by hand |
| dbt staging | `stg_blocks`, `stg_transactions` (views) in `models/staging/` |
| dbt marts | `daily_network_activity`, `top_senders` (tables), `daily_gas` (incremental, merge) in `models/marts/` |
| Tests and docs | unique, not_null, accepted_values, relationships, descriptions in `models/schema.yml` |
| dbt connection | `profiles.yml` attaches the lake as `eth_lake` |

## Run it yourself

1. Clone the repository and open the folder in [Rosetta DBT Studio](https://rosettadb.io/download-dbtstudio.html).
2. Create the lake: Data Lake, New DataLake, DuckLake with a DuckDB catalog at `lake/eth_catalog.duckdb` and data in `lake/data/`, named `eth_lake`.
3. Run the files in `sql/` in order, in the SQL Editor.
4. Run `dbt build`.

Or in plain DuckDB, from the project folder:

```sql
install ducklake; install httpfs;
attach 'ducklake:lake/eth_catalog.duckdb' as eth_lake (data_path 'lake/data/');
use eth_lake;
-- then the statements in sql/, in order
```

You need about 3 GB of free disk. No cloud account: the data is read from a public S3 bucket.

## Data

AWS Public Blockchain Data, Ethereum blocks and transactions as Parquet, one folder per day: `s3://aws-public-blockchain/v1.0/eth/`
https://registry.opendata.aws/aws-public-blockchain/

## Tools

[DuckDB](https://duckdb.org/), [DuckLake](https://ducklake.select/), [dbt Core](https://github.com/dbt-labs/dbt-core) with [dbt-duckdb](https://github.com/duckdb/dbt-duckdb), [Rosetta DBT Studio](https://github.com/rosettadb/dbt-studio). All open source.
