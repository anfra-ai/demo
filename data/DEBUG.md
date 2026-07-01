# data

Bundled data for this anfra project.

## `demo_ecommerce.duckdb`

Backs the `demo_ecommerce` dataset (data source `duckdb_demo`, configured in
`.anfra/data_sources.yml`). Holds 8 tables in the `demo` schema:

| Table | Rows |
|-------|------|
| categories | 25 |
| cities | 19 |
| countries | 10 |
| merchants | 759 |
| products | 8,731 |
| users | 10,264 |
| orders | 33,013 |
| order_items | 54,783 |

Inspect it directly with the DuckDB CLI:

```bash
duckdb data/demo_ecommerce.duckdb "SELECT count(*) FROM demo.orders"
```
