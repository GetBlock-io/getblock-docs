---
description: >-
  Robinhood Chain token metadata and decoded Uniswap v3/v4, Doppler, Flap,
  LetsCash, Long, Nox, Noxa, Pons, and Varo launchpad activity.
---

# Robinhood Chain - Market SQL

The **Robinhood Chain** dataset provides token launches, bonding-curve trading, migrations, Uniswap v3 and v4 pools and trades, and related token metadata.

<table data-search="false"><thead><tr><th>Property</th><th>Value</th></tr></thead><tbody><tr><td>Database name</td><td><code>robinhood</code></td></tr><tr><td>Tables</td><td>25</td></tr><tr><td>Access</td><td>Subscribed to separately</td></tr></tbody></table>

### Tables

<table data-search="false"><thead><tr><th>Table</th><th>Description</th></tr></thead><tbody><tr><td><code>doppler_creations</code></td><td>Doppler launch and pool creation events with assets, initializer, fees and token metadata.</td></tr><tr><td><code>doppler_trading</code></td><td>Query-time view of decoded Doppler pool trading activity.</td></tr><tr><td><code>flap_creations</code></td><td>Flap token and launch creation events with creator and transaction context.</td></tr><tr><td><code>flap_curve_trades</code></td><td>Flap bonding-curve trades with amounts, direction, curve state and fees.</td></tr><tr><td><code>flap_migrations</code></td><td>Flap liquidity and token migration events.</td></tr><tr><td><code>flap_trading</code></td><td>Query-time view of consolidated Flap trading activity.</td></tr><tr><td><code>letscash_creations</code></td><td>LetsCash token and launch creation events.</td></tr><tr><td><code>letscash_trading</code></td><td>Query-time view of decoded LetsCash trading activity.</td></tr><tr><td><code>long_creations</code></td><td>Long launchpad token creation events.</td></tr><tr><td><code>long_trading</code></td><td>Query-time view of decoded Long launchpad trading activity.</td></tr><tr><td><code>nox_creations</code></td><td>Nox token and launch creation events.</td></tr><tr><td><code>nox_trading</code></td><td>Query-time view of decoded Nox trading activity.</td></tr><tr><td><code>noxa_creations</code></td><td>Noxa token and launch creation events.</td></tr><tr><td><code>noxa_trading</code></td><td>Query-time view of decoded Noxa trading activity.</td></tr><tr><td><code>pons_creations</code></td><td>Pons token and bonding-curve creation events.</td></tr><tr><td><code>pons_curve_trades</code></td><td>Pons bonding-curve trades with amounts, direction, curve state and transaction context.</td></tr><tr><td><code>pons_migrations</code></td><td>Pons token and liquidity migration events.</td></tr><tr><td><code>pons_trading</code></td><td>Query-time view of consolidated Pons trading activity.</td></tr><tr><td><code>tokens</code></td><td>ERC-20 identities with descriptive metadata, creator and creation context.</td></tr><tr><td><code>uniswap_v3_pools</code></td><td>Uniswap v3 token pairs with pool address, fee tier and tick spacing.</td></tr><tr><td><code>uniswap_v3_trades</code></td><td>Uniswap v3 swaps with amounts, price, liquidity, tick, gas and transaction context.</td></tr><tr><td><code>uniswap_v4_pools</code></td><td>Uniswap v4 currencies, fees, hooks and initial pool state.</td></tr><tr><td><code>uniswap_v4_trades</code></td><td>Uniswap v4 swaps with amounts, fees, price, liquidity, tick, gas and transaction context.</td></tr><tr><td><code>varo_creations</code></td><td>Varo token and launch creation events.</td></tr><tr><td><code>varo_trading</code></td><td>Query-time view of decoded Varo trading activity.</td></tr></tbody></table>

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

{% tab title="Javascript" %}
{% code overflow="wrap" %}
```js
import { createClient } from '@clickhouse/client'

const client = createClient({
  host: 'https://market-sql.eu-central-1.getblock.io',
  username: process.env.GETBLOCK_USER_ID,
  password: process.env.GETBLOCK_API_KEY,
  database: 'robinhood'
})

const rows = await client.query({
  query: 'SELECT * FROM uniswap_v4_trades LIMIT 5',
  format: 'JSONEachRow'
})

console.log(await rows.json())
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
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

print(client.query_df('SELECT * FROM uniswap_v4_trades LIMIT 5')
```
{% endcode %}
{% endtab %}

{% tab title="GO" %}
{% code overflow="wrap" %}
```go
package main

import (
	"crypto/tls"
	"fmt"
	"log"
	"os"

	"github.com/ClickHouse/clickhouse-go/v2"
)

func main() {
	db := clickhouse.OpenDB(&clickhouse.Options{
		Addr:     []string{"market-sql.eu-central-1.getblock.io:443"},
		Protocol: clickhouse.HTTP,
		TLS:      &tls.Config{},
		Auth: clickhouse.Auth{
			Database: "robinhood",
			Username: os.Getenv("GETBLOCK_USER_ID"),
			Password: os.Getenv("GETBLOCK_API_KEY"),
		},
	})
	defer db.Close()

	rows, err := db.Query("SELECT * FROM uniswap_v4_trades LIMIT 5")
	if err != nil {
		log.Fatal(err)
	}
	defer rows.Close()

	for rows.Next() {
		var blockTime, direction, baseToken, quoteToken any
		if err := rows.Scan(&blockTime, &direction, &baseToken, &quoteToken); err != nil {
			log.Fatal(err)
		}
		fmt.Println(blockTime, direction, baseToken, quoteToken)
	}
	if err := rows.Err(); err != nil {
		log.Fatal(err)
	}
}
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code overflow="wrap" %}
```rs
use clickhouse::Client;
use std::env;
use tokio::io::AsyncBufReadExt;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::default()
        .with_url("https://market-sql.eu-central-1.getblock.io:443")
        .with_user(env::var("GETBLOCK_USER_ID")?)
        .with_password(env::var("GETBLOCK_API_KEY")?)
        .with_database("robinhood");

    let mut lines = client
        .query("SELECT * FROM uniswap_v4_trades LIMIT 5")
        .fetch_bytes("JSONEachRow")?
        .lines();

    while let Some(line) = lines.next_line().await? {
        println!("{line}");
    }

    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Response Sample

{% code overflow="wrap" %}
```bash
{
    "pool_id": "0xdb2c20421239d46bb30a7a73029b7f9b7f166489bfb972057d33cbd7249413a5",
    "sender": "0x53bf6b0684ec7ef91e1387da3d1a1769bc5a6f77",
    "amount0": 49997492603174,
    "amount1": -1000000000000000000,
    "sqrt_price_x96": 1579888788696945134562238999968156,
    "liquidity": 50000000000000,
    "tick": 198020,
    "fee": 3000,
    "event_id": "0x16a7f8e5dedb48adcc107489db7b4bee202ed28f4117be185322fdaf09294d20-0",
    "block_number": 9507,
    "log_index": 0,
    "transaction_index": 1,
    "contract_address": "0x8366a39cc670b4001a1121b8f6a443a643e40951",
    "block_hash": "0x10a35beefa196065fa31cf332847d62b54c7f373d27d84b4fa9788bdba53b00a",
    "block_timestamp": "2026-05-22 21:15:34",
    "gas_used": 219766,
    "gas_limit": 1125899906842624,
    "base_fee_per_gas": 100010000,
    "transaction_hash": "0x16a7f8e5dedb48adcc107489db7b4bee202ed28f4117be185322fdaf09294d20",
    "transaction_from": "0x9701fb0ade1e269c8f64ec0c7b3cfadb31a13a52",
    "transaction_to": "0x53bf6b0684ec7ef91e1387da3d1a1769bc5a6f77",
    "transaction_value": 0,
    "transaction_gas": 286997,
    "transaction_nonce": 45,
    "max_fee_per_gas": 200320001,
    "max_priority_fee_per_gas": 1,
    "inserted_at": "2026-08-05 07:30:10"
}
```
{% endcode %}

## Next steps

* [**API Reference**](./) — endpoint, authentication, and limits
* [**Querying Market SQL**](../querying-market-sql.md) — datasets, queries, and result formats
