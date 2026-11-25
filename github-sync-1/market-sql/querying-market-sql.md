---
description: >-
  How a Market SQL request works — choosing a dataset, writing the query, and
  choosing how results are returned.
---

# Querying Market SQL

A **Market SQL** request describes three things:

1. the dataset to query,
2. the SQL statement to run,
3. and the format the results should be returned in.

This page explains those three choices. For the endpoint, parameters, limits, and the full table catalog, see the [**API Reference**](api-reference/).

### 1. The dataset

The `database` parameter selects the dataset. Your API key must have an active trial or subscription for that dataset.

<table data-search="false"><thead><tr><th>Dataset</th><th>Database name</th><th>Tables</th></tr></thead><tbody><tr><td><a href="api-reference/solana.md">Solana</a></td><td><code>solana</code></td><td>21</td></tr><tr><td><a href="api-reference/polymarket.md">Polymarket</a></td><td><code>polymarket</code></td><td>3</td></tr><tr><td><a href="api-reference/hyperliquid.md">Hyperliquid</a></td><td><code>hyperliquid</code></td><td>4</td></tr><tr><td><a href="api-reference/robinhood.md">Robinhood Chain</a></td><td><code>robinhood</code></td><td>25</td></tr></tbody></table>

{% hint style="info" %}
The `database` parameter sets the default database for the request, so tables are referenced by name without a prefix: `FROM pumpswap_all_swaps`, not `FROM solana.pumpswap_all_swaps`.
{% endhint %}

### 2. The query

The request body is a single, read-only SQL statement written in ClickHouse SQL. You can filter with `WHERE`, aggregate with `GROUP BY`, sort, limit, and join tables within the same dataset.

```sql
SELECT block_time, direction, base_token, quote_token
FROM pumpswap_all_swaps
LIMIT 5;
```

Because filtering, aggregation, and joins run server-side, you receive only the rows your question needs rather than downloading raw records and processing them locally.

#### Execution limits

Every query runs within the execution limit of your access plan:

<table data-search="false"><thead><tr><th>Plan</th><th>Concurrent queries</th><th>Execution limit</th></tr></thead><tbody><tr><td>Free trial</td><td>1</td><td>10 seconds</td></tr><tr><td>24-hour trial</td><td>1</td><td>10 seconds</td></tr><tr><td>Monthly</td><td>Higher query limits</td><td>60 seconds</td></tr><tr><td>Custom</td><td>Custom query capacity</td><td>Custom</td></tr></tbody></table>

{% hint style="info" %}
1. A query that runs longer than the execution limit is stopped. Filter on time or key columns, select only the columns you need, and add `LIMIT` while exploring.
2. On the trial plans, only one query can run at a time. A second request returns `TOO_MANY_SIMULTANEOUS_QUERIES`. A query keeps running on the server even if your client stops waiting for it, so a slow query can block the next one: wait for it to finish, then retry. Filtering on `block_time` keeps queries short.
{% endhint %}

#### Query parameters

Pass values separately from the SQL with ClickHouse query parameters instead of building the query string yourself. Write a placeholder as `{name:Type}` in the SQL, and send its value as a `param_<name>` URL parameter:

{% code overflow="wrap" %}
```bash
curl --fail-with-body \
  --user "<USER-ID>:<API-KEY>" \
  "https://market-sql.eu-central-1.getblock.io?database=solana&default_format=JSONEachRow&param_start=2026-11-22%2000:00:00&param_end=2026-11-23%2000:00:00" \
  --data-binary "SELECT count() AS migrations FROM pfamm_migrations WHERE block_time >= {start:DateTime} AND block_time < {end:DateTime}"
```
{% endcode %}

### 3. The result format

The `default_format` parameter sets how rows are returned. All examples in this documentation use `JSONEachRow`, which returns one JSON object per line:

{% code overflow="wrap" %}
```json
{"block_time":"2025-10-09 17:00:00","direction":"B","base_token":"HdbQVdtTh3gFuvmvVqHCh37w9ADPCiE9f24EX63npump","quote_token":"So11111111111111111111111111111111111111112"}
{"block_time":"2025-10-09 17:00:00","direction":"S","base_token":"So11111111111111111111111111111111111111112","quote_token":"2oyFNVveXsgPZGgwSPMSASriv59M5ZqZ9CCewanK38Q8"}
{"block_time":"2025-10-09 17:00:00","direction":"B","base_token":"So11111111111111111111111111111111111111112","quote_token":"5Dyr4rWsqGxJEtVRUM7HzgS6UQE93Babqot8mgevvHPh"}
{"block_time":"2025-10-09 17:00:00","direction":"S","base_token":"2ktfSdqv5Te5XRFo4Do5U21RNGKiyuWbQatXp7p8pump","quote_token":"So11111111111111111111111111111111111111112"}
{"block_time":"2025-10-09 17:00:00","direction":"S","base_token":"So11111111111111111111111111111111111111112","quote_token":"2rP2qcdjv3LFYvwU9Rp89TDm7EAf3Zv9iuRXP6Uckw2T"}
```
{% endcode %}

A `FORMAT` clause at the end of the SQL statement takes precedence over `default_format`.  The JavaScript client `@clickhouse/client` sets the format through its `format` option. In Python, pass `default_format` as a URL parameter, as in the examples.

{% hint style="warning" %}
### Research, not live delivery

Market SQL is built for research and analytics. It does not push updates and is not a live trading signal feed or WebSocket service.

Applications that need continuously updated Solana prices, trades, or candles should use [**Solana Market Data**](../solana-market-data/overview.md), and use Market SQL to research and backtest the same markets.
{% endhint %}

## Next steps

* [**API Reference**](api-reference/) — endpoints, authentication, parameters, and every table
* [**Getting Started**](getting-started.md) — activating a dataset and running a first query
* [**Dashboard**](https://account.getblock.io/products/sql) — browse datasets and tables before you query
