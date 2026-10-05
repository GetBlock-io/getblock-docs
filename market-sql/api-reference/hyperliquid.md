---
description: >-
  Exchange and market activity for derivatives analytics, backtesting, and
  quantitative workflows.
---

# Hyperliquid - Market SQL

The **Hyperliquid** dataset provides order fills, perpetual-market activity, wallet positions, and aggregated order analytics.

<table data-search="false"><thead><tr><th>Property</th><th>Value</th></tr></thead><tbody><tr><td>Database name</td><td><code>hyperliquid</code></td></tr><tr><td>Tables</td><td>4</td></tr><tr><td>Access</td><td>Subscribed to separately</td></tr></tbody></table>

### Tables

<table data-search="true"><thead><tr><th>Table</th><th>Description</th></tr></thead><tbody><tr><td><code>agg_fulfilled_order</code></td><td>HyperLiquid fills aggregated by fulfilled order, including size, volume, PnL and wallet.</td></tr><tr><td><code>raw_node_fills_by_block</code></td><td>Raw HyperLiquid block fills with price, size, side, fee, PnL, TWAP, builder and liquidation context.</td></tr><tr><td><code>view_perpetual_wallet</code></td><td>Current perpetual-wallet analytics including realized PnL, volume, win counts and ROI distributions.</td></tr><tr><td><code>view_wallet_position</code></td><td>Current wallet positions by market with signed size and last update time.</td></tr></tbody></table>

### Example request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --fail-with-body \
  --user "<USER-ID>:<API-KEY>" \
  "https://market-sql.eu-central-1.getblock.io?database=hyperliquid&default_format=JSONEachRow" \
  --data-binary "SELECT * FROM view_perpetual_wallet LIMIT 5"
```
{% endcode %}
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
    database='hyperliquid',
    secure=True,
)

print(client.query_df('SELECT * FROM view_perpetual_wallet LIMIT 5'))
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Historical coverage varies by dataset. Browse table descriptions in the [**Datasets**](https://account.getblock.io/products/sql) tab of your dashboard.
{% endhint %}

## Next steps

* [**API Reference**](./) — endpoint, authentication, and limits
* [**Querying Market SQL**](../querying-market-sql.md) — datasets, queries, and result formats
