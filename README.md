# anfra demo

An **anfra** (Agentic Analytics Infrastructure) project — local-first, agent-friendly
analytics defined as AML (datasets, models, metrics) and queried with AQL. Everything
runs locally: no database server to stand up, no cloud credentials.

## Datasets

| Dataset | Backing data source | Data |
|---------|--------------------|------|
| `demo_ecommerce` | `duckdb_demo` (DuckDB) | `data/demo_ecommerce.duckdb` — bundled, ~108k rows |

`demo_ecommerce` is the first dataset in this repo. It models a small ecommerce store
(categories, cities, countries, merchants, order_items, orders, products, users) with
relationships and metrics such as revenue and order/user counts. Its data ships as a
committed DuckDB file, so the dataset works the moment you clone the repo.

## Usage

Run commands from the repo root so relative data-source paths (e.g.
`data/demo_ecommerce.duckdb`) resolve.

Validate the whole AML repo:

```bash
anfra validate
```

Run an AQL query against a dataset (inline, or piped via stdin):

```bash
anfra query --dataset demo_ecommerce --aql 'count(demo_users.id)'

cat examples/queries/metrics.aql | anfra query --dataset demo_ecommerce
```

Add `--generate` to print the compiled SQL instead of running it.
