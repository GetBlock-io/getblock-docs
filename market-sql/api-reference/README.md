---
description: >-
  Full reference for Market SQL: endpoint, authentication, parameters, limits,
  and every table in each dataset, with examples in cURL, JavaScript, and
  Python.
---

# API Reference

This section documents how to connect to Market SQL and which tables each dataset exposes.

Market SQL has a single endpoint. What you receive depends on the [**dataset**](./#datasets) you select and the SQL you send. Each dataset has its own page below listing every table and what it contains.

### Quickstart

Inspect five PumpSwap trades on Solana:

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --fail-with-body \
  --user "<USER-ID>:<API-KEY>" \
  "https://market-sql.eu-central-1.getblock.io?database=solana&default_format=JSONEachRow" \
  --data-binary "SELECT block_time, direction, base_token, quote_token FROM pumpswap_all_swaps LIMIT 5"
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
```javascript
import { createClient } from '@clickhouse/client'

const client = createClient({
  host: 'https://market-sql.eu-central-1.getblock.io',
  username: process.env.GETBLOCK_USER_ID,
  password: process.env.GETBLOCK_API_KEY,
  database: 'solana'
})

const rows = await client.query({
  query: 'SELECT block_time, direction, base_token, quote_token FROM pumpswap_all_swaps LIMIT 5',
  format: 'JSONEachRow'
})

console.log(await rows.json())
```
{% endtab %}

{% tab title="Python" %}
```python
import os
import clickhouse_connect

client = clickhouse_connect.get_client(
    host='market-sql.eu-central-1.getblock.io',
    port=443,
    username=os.environ['GETBLOCK_USER_ID'],
    password=os.environ['GETBLOCK_API_KEY'],
    database='solana',
    secure=True,
)

print(client.query_df(
    'SELECT block_time, direction, base_token, quote_token FROM pumpswap_all_swaps LIMIT 5'
))
```
{% endtab %}
{% endtabs %}

### Response

{% code overflow="wrap" %}
```json
{"block_time":"2025-10-09 17:00:00","direction":"B","base_token":"HdbQVdtTh3gFuvmvVqHCh37w9ADPCiE9f24EX63npump","quote_token":"So11111111111111111111111111111111111111112"}
{"block_time":"2025-10-09 17:00:00","direction":"S","base_token":"So11111111111111111111111111111111111111112","quote_token":"2oyFNVveXsgPZGgwSPMSASriv59M5ZqZ9CCewanK38Q8"}
{"block_time":"2025-10-09 17:00:00","direction":"B","base_token":"So11111111111111111111111111111111111111112","quote_token":"5Dyr4rWsqGxJEtVRUM7HzgS6UQE93Babqot8mgevvHPh"}
{"block_time":"2025-10-09 17:00:00","direction":"S","base_token":"2ktfSdqv5Te5XRFo4Do5U21RNGKiyuWbQatXp7p8pump","quote_token":"So11111111111111111111111111111111111111112"}
{"block_time":"2025-10-09 17:00:00","direction":"S","base_token":"So11111111111111111111111111111111111111112","quote_token":"2rP2qcdjv3LFYvwU9Rp89TDm7EAf3Zv9iuRXP6Uckw2T"}

```
{% endcode %}

### Endpoint

{% code overflow="wrap" %}
```bash
https://market-sql.eu-central-1.getblock.io
```
{% endcode %}

### Authentication

Credentials go in **HTTP Basic Auth** on every request, not in the URL.

<table data-search="false"><thead><tr><th>Field</th><th>Value</th></tr></thead><tbody><tr><td>Username</td><td>Your GetBlock user ID</td></tr><tr><td>Password</td><td>A GetBlock API key in <code>gb_…</code> format</td></tr></tbody></table>

### Parameters

<table data-search="false"><thead><tr><th>Parameter</th><th>Type</th><th>Required</th><th>Description</th></tr></thead><tbody><tr><td><code>database</code></td><td>string</td><td>Yes</td><td>Dataset to query: <code>solana</code>, <code>polymarket</code>, <code>hyperliquid</code>, or <code>robinhood</code>.</td></tr><tr><td><code>default_format</code></td><td>string</td><td>No</td><td>Output format, for example <code>JSONEachRow</code>. A <code>FORMAT</code> clause in the SQL takes precedence.</td></tr></tbody></table>

### Datasets

<table data-search="false"><thead><tr><th>Dataset</th><th>Description</th><th>Tables</th></tr></thead><tbody><tr><td><a href="solana.md"><code>solana</code></a></td><td>Swaps, launches, transfers, funding, blocks, and Jito tips</td><td>21</td></tr><tr><td><a href="polymarket.md"><code>polymarket</code></a></td><td>Prediction-market fills, events, and markets</td><td>3</td></tr><tr><td><a href="hyperliquid.md"><code>hyperliquid</code></a></td><td>Perpetual fills, wallet analytics, and positions</td><td>4</td></tr><tr><td><a href="robinhood.md"><code>robinhood</code></a></td><td>Launchpads, Uniswap v3/v4, and token metadata on Robinhood Chain</td><td>25</td></tr></tbody></table>

### Choosing a dataset

<table data-search="false"><thead><tr><th>If you need</th><th>Use</th></tr></thead><tbody><tr><td>Solana DEX swaps, token launches, and migrations</td><td><code>solana</code></td></tr><tr><td>Wallet funding paths and token transfers on Solana</td><td><code>solana</code></td></tr><tr><td>Perpetual fills, wallet PnL, and open positions</td><td><code>hyperliquid</code></td></tr><tr><td>Launchpad and Uniswap activity on Robinhood Chain</td><td><code>robinhood</code></td></tr><tr><td>Prediction-market fills and outcomes</td><td><code>polymarket</code></td></tr></tbody></table>

### Concepts that apply to every query

1. **One dataset per plan:** Access is purchased per dataset. A query to a dataset your API key has no access to is rejected.
2. **Read-only:** The gateway runs every query under a managed, read-only identity. Only statements that read data are accepted.
3. **Execution limits:** Trials allow one concurrent query and 10 seconds of execution. Monthly access allows 60 seconds with higher query limits.
4. **Errors:** A failed query returns a non-2xx HTTP status with the error message in the response body. Use `curl --fail-with-body` to see the message while still failing the command.

| Symptom                             | Cause                                                                       | Resolution                                                          |
| ----------------------------------- | --------------------------------------------------------------------------- | ------------------------------------------------------------------- |
| Authentication is rejected          | The user ID and API key belong to different accounts, or the key is invalid | Use the user ID of the account that owns the key                    |
| Access to a dataset is denied       | No active trial or subscription for that `database`                         | Activate the dataset on the **Pricing** tab                         |
| A query stops before finishing      | The query exceeded the plan's execution limit                               | Add filters, select fewer columns, add `LIMIT`, or upgrade the plan |
| A driver or IDE cannot connect      | The client uses the native TCP protocol                                     | Connect over HTTPS on port `443`                                    |
| A second query fails during a trial | Trials allow one concurrent query                                           | Run queries one after another                                       |
