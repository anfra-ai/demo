# anfra demo

An **anfra** (Agentic Analytics Infrastructure) project — local-first, agent-friendly
analytics defined as AML (datasets, models, metrics) and queried with AQL. Everything
runs locally: no database server to stand up, no cloud credentials.

## Install

Install the `anfra` binary:

```sh
curl -fsSL https://raw.githubusercontent.com/holistics/anfra/main/install.sh | bash
```

The installer downloads the latest release for your platform, places the `anfra`
binary in `~/.anfra/bin`, and prints the line to add it to your `PATH`.
Supported platforms: linux (x64/arm64) and macOS (x64/arm64).

You can configure the installer with environment variables:

- `ANFRA_INSTALL_DIR` — install somewhere else (default: `~/.anfra/bin`)
- `ANFRA_VERSION` — install a specific version, e.g. `0.1.0` (default: latest)

Update an existing install with `anfra update` (or `anfra update --check` to check
without installing).

## Datasets

| Dataset | Backing data source | Data |
|---------|--------------------|------|
| `demo_ecommerce` | `duckdb_demo` (DuckDB) | `data/demo_ecommerce.duckdb` — bundled, ~108k rows |

`demo_ecommerce` is the first dataset in this repo. It models a small ecommerce store
(categories, cities, countries, merchants, order_items, orders, products, users) with
relationships and metrics such as revenue and order/user counts. Its data ships as a
committed DuckDB file, so the dataset works the moment you clone the repo.

## Skills

Agent skills for working with anfra — authoring AML and writing AQL against a repo like this one — live in [`anfra-ai/skills`](https://github.com/anfra-ai/skills).
Install them through whatever skill mechanism your coding agent supports.

For Claude Code, add the marketplace and install:

```
/plugin marketplace add anfra-ai/skills
/plugin install <name>@anfra-skills
```

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

### Optional: keep servers warm for faster commands

Each `query` or `validate` call spins up its query sidecars from cold. If you're
running several commands (or letting an agent drive), start a warm server once:

```bash
anfra serve
```

It keeps the sidecars alive and exposes a local endpoint that subsequent `anfra` CLI calls and agents reuse, so later commands skip cold-start and return noticeably faster. Check whether one is already running with `anfra status`.