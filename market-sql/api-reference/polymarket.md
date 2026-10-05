---
description: >-
  Market, order, and trading data for prediction-market research and strategy
  analysis.
---

# Polymarket - Market SQL

The **Polymarket** dataset provides decoded order fills from Polygon, together with event and market metadata such as titles, categories, outcomes, status, and resolution.

<table data-search="false"><thead><tr><th>Property</th><th>Value</th></tr></thead><tbody><tr><td>Database name</td><td><code>polymarket</code></td></tr><tr><td>Tables</td><td>3</td></tr><tr><td>Access</td><td>Subscribed to separately</td></tr></tbody></table>

### Tables

<table data-search="true"><thead><tr><th>Table</th><th>Description</th></tr></thead><tbody><tr><td><code>polymarket_order_filled_v3</code></td><td>Decoded Polygon maker and taker fill legs with outcome tokens, USDC amounts, fees and block context.</td></tr><tr><td><code>raw_event_meta</code></td><td>Polymarket event titles, slugs, dates, categories, status, liquidity and resolution metadata.</td></tr><tr><td><code>raw_market_meta</code></td><td>Polymarket questions, outcomes, token identifiers, trading status and resolution metadata.</td></tr></tbody></table>

### Example request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --fail-with-body \
  --user "<USER-ID>:<API-KEY>" \
  "https://market-sql.eu-central-1.getblock.io?database=polymarket&default_format=JSONEachRow" \
  --data-binary "SELECT * FROM raw_market_meta LIMIT 5"
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
    database='polymarket',
    secure=True,
)

print(client.query_df('SELECT * FROM raw_market_meta LIMIT 5'))
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Historical coverage varies by dataset. Browse table descriptions in the [**Datasets**](https://account.getblock.io/products/sql) tab of your dashboard.
{% endhint %}

## Next steps

* [**API Reference**](./) — endpoint, authentication, and limits
* [**Querying Market SQL**](../querying-market-sql.md) — datasets, queries, and result formats
