---
description: >-
  Robinhood Chain token metadata and decoded Uniswap v3/v4, Doppler, Flap,
  LetsCash, Long, Nox, Noxa, Pons, and Varo launchpad activity.
---

# Robinhood Chain - Market SQL

The **Robinhood Chain** dataset provides token launches, bonding-curve trading, migrations, Uniswap v3 and v4 pools and trades, and related token metadata.

<table data-search="false"><thead><tr><th>Property</th><th>Value</th></tr></thead><tbody><tr><td>Database name</td><td><code>robinhood</code></td></tr><tr><td>Tables</td><td>25</td></tr><tr><td>Access</td><td>Subscribed to separately</td></tr></tbody></table>

### Tables

<table data-search="true"><thead><tr><th>Table</th><th>Description</th></tr></thead><tbody><tr><td><code>doppler_creations</code></td><td>Doppler launch and pool creation events with assets, initializer, fees and token metadata.</td></tr><tr><td><code>doppler_trading</code></td><td>Query-time view of decoded Doppler pool trading activity.</td></tr><tr><td><code>flap_creations</code></td><td>Flap token and launch creation events with creator and transaction context.</td></tr><tr><td><code>flap_curve_trades</code></td><td>Flap bonding-curve trades with amounts, direction, curve state and fees.</td></tr><tr><td><code>flap_migrations</code></td><td>Flap liquidity and token migration events.</td></tr><tr><td><code>flap_trading</code></td><td>Query-time view of consolidated Flap trading activity.</td></tr><tr><td><code>letscash_creations</code></td><td>LetsCash token and launch creation events.</td></tr><tr><td><code>letscash_trading</code></td><td>Query-time view of decoded LetsCash trading activity.</td></tr><tr><td><code>long_creations</code></td><td>Long launchpad token creation events.</td></tr><tr><td><code>long_trading</code></td><td>Query-time view of decoded Long launchpad trading activity.</td></tr><tr><td><code>nox_creations</code></td><td>Nox token and launch creation events.</td></tr><tr><td><code>nox_trading</code></td><td>Query-time view of decoded Nox trading activity.</td></tr><tr><td><code>noxa_creations</code></td><td>Noxa token and launch creation events.</td></tr><tr><td><code>noxa_trading</code></td><td>Query-time view of decoded Noxa trading activity.</td></tr><tr><td><code>pons_creations</code></td><td>Pons token and bonding-curve creation events.</td></tr><tr><td><code>pons_curve_trades</code></td><td>Pons bonding-curve trades with amounts, direction, curve state and transaction context.</td></tr><tr><td><code>pons_migrations</code></td><td>Pons token and liquidity migration events.</td></tr><tr><td><code>pons_trading</code></td><td>Query-time view of consolidated Pons trading activity.</td></tr><tr><td><code>tokens</code></td><td>ERC-20 identities with descriptive metadata, creator and creation context.</td></tr><tr><td><code>uniswap_v3_pools</code></td><td>Uniswap v3 token pairs with pool address, fee tier and tick spacing.</td></tr><tr><td><code>uniswap_v3_trades</code></td><td>Uniswap v3 swaps with amounts, price, liquidity, tick, gas and transaction context.</td></tr><tr><td><code>uniswap_v4_pools</code></td><td>Uniswap v4 currencies, fees, hooks and initial pool state.</td></tr><tr><td><code>uniswap_v4_trades</code></td><td>Uniswap v4 swaps with amounts, fees, price, liquidity, tick, gas and transaction context.</td></tr><tr><td><code>varo_creations</code></td><td>Varo token and launch creation events.</td></tr><tr><td><code>varo_trading</code></td><td>Query-time view of decoded Varo trading activity.</td></tr></tbody></table>

### Example request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --fail-with-body \
  --user "<USER-ID>:<API-KEY>" \
  "https://market-sql.eu-central-1.getblock.io?database=robinhood&default_format=JSONEachRow" \
  --data-binary "SELECT * FROM uniswap_v4_trades LIMIT 5"
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
    database='robinhood',
    secure=True,
)

print(client.query_df('SELECT * FROM uniswap_v4_trades LIMIT 5'))
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Historical coverage varies by dataset. Browse table descriptions in the [**Datasets**](https://account.getblock.io/products/sql) tab of your dashboard.
{% endhint %}

## Next steps

* [**API Reference**](./) — endpoint, authentication, and limits
* [**Querying Market SQL**](../querying-market-sql.md) — datasets, queries, and result formats
