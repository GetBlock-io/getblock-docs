---
description: >-
  Blocks and trading activity on Solana across Pump.fun, PumpSwap, Meteora,
  Raydium, and Jito.
---

# Solana - Market SQL

The **Solana** dataset provides swaps and launches across Pump.fun, PumpSwap, Raydium, and Meteora, alongside token transfers, SOL funding activity, blocks, timestamps, Jito tips, and other market infrastructure data.

<table data-search="false"><thead><tr><th>Property</th><th>Value</th></tr></thead><tbody><tr><td>Database name</td><td><code>solana</code></td></tr><tr><td>Tables</td><td>21</td></tr><tr><td>Access</td><td>Subscribed to separately</td></tr></tbody></table>

### Tables

<table data-search="false"><thead><tr><th>Table</th><th>Description</th></tr></thead><tbody><tr><td><code>jito_tips</code></td><td>Jito tip transfers with sender, recipient account, amount and Solana transaction context.</td></tr><tr><td><code>max_caps</code></td><td>Precomputed maximum token market-cap observations in SOL and USDC. </td></tr><tr><td><code>meteora_dynamic_bonding_swaps</code></td><td>Meteora dynamic bonding-curve swaps with amounts, reserves, fees and routing context.</td></tr><tr><td><code>meteora_swaps</code></td><td>Meteora DLMM swaps with token amounts, bin movement, fees and transaction details.</td></tr><tr><td><code>pfamm_migrations</code></td><td>Pump.fun AMM migrations with migrated token and SOL amounts, pools and wallets.</td></tr><tr><td><code>pumpfun_all_swaps</code></td><td>Decoded Pump.fun bonding-curve swaps with direction, amounts, virtual reserves and fees.</td></tr><tr><td><code>pumpfun_amm_admin_set_coin_creator</code></td><td>Pump.fun AMM administrative events that change a pool's coin creator.</td></tr><tr><td><code>pumpfun_creator_fee_distributions</code></td><td>Pump.fun creator-fee distribution events with recipient, mint, amount and transaction context.</td></tr><tr><td><code>pumpfun_token_creation</code></td><td>Pump.fun token creation events with mint, creator and initial bonding-curve metadata.</td></tr><tr><td><code>pumpfun_v2_swaps</code></td><td>Pump.fun v2 swap activity with token amounts, fee components and transaction context.</td></tr><tr><td><code>pumpswap_all_swaps</code></td><td>PumpSwap AMM swaps with reserves, fee components, creator, cashback and buyback fields.</td></tr><tr><td><code>raydium_all_swaps</code></td><td>Unified decoded swap activity across supported Raydium programs.</td></tr><tr><td><code>raydium_cpmm_swaps</code></td><td>Raydium constant-product pool swaps with token amounts, pools, fees and transaction context.</td></tr><tr><td><code>raydium_launchpad_cpmm_migrations</code></td><td>Raydium Launchpad migrations into constant-product pools.</td></tr><tr><td><code>raydium_launchpad_migrations</code></td><td>Raydium Launchpad token and liquidity migration events.</td></tr><tr><td><code>raydium_launchpad_swaps</code></td><td>Raydium Launchpad swaps with trade direction, amounts, curve state and fees.</td></tr><tr><td><code>raydium_launchpad_token_creation</code></td><td>Raydium Launchpad token creation events and initial launch configuration.</td></tr><tr><td><code>sol_top_ups</code></td><td>SOL funding transfers used to trace wallet top-up paths.</td></tr><tr><td><code>solana_blocks</code></td><td>Solana block and slot coverage with production timestamps.</td></tr><tr><td><code>token_transfers</code></td><td>Decoded token transfer activity with assets, amounts, wallets and transaction context.</td></tr><tr><td><code>tx_timestamps</code></td><td>Reusable transaction-to-block timestamp lookup data.</td></tr></tbody></table>

{% hint style="info" %}
### Coverage differs by table.

For example, `pumpfun_token_creation` starts in January 2024, while `pfamm_migrations` and `pumpswap_all_swaps` start on 25 November 2025. Check a table's range before choosing a study window:

```sql
SELECT min(block_time), max(block_time) FROM pfamm_migrations
```
{% endhint %}

#### Columns

To list a table's columns and types, run `DESCRIBE TABLE`:

```bash
curl --fail-with-body \
  --user "<USER-ID>:<API-KEY>" \
  "https://market-sql.eu-central-1.getblock.io?database=solana&default_format=PrettyCompact" \
  --data-binary "DESCRIBE TABLE pumpswap_all_swaps"
```

{% hint style="warning" %}
`pumpfun_token_creation` stores `mint` and `creator` as `FixedString(48)`, while other tables such as `pfamm_migrations` and `pumpswap_all_swap`\` use \`String\`. Convert with `toString(mint)` before joining or filtering with `IN`.
{% endhint %}

### Example request

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

{% tab title="Javascript" %}
{% code overflow="wrap" %}
```js
import { createClient } from '@clickhouse/client'

const client = createClient({
  host: 'https://market-sql.eu-central-1.getblock.io',
  username: process.env.GETBLOCK_USER_ID,
  password: process.env.GETBLOCK_API_KEY,
  database: 'solana'
})

const rows = await client.query({
  query: 'SELECT * FROM pumpswap_all_swaps LIMIT 10',
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
import requests

response = requests.post(
    "https://market-sql.eu-central-1.getblock.io",
    params={"database": "solana", "default_format": "JSONEachRow"},
    auth=(os.environ["GETBLOCK_USER_ID"], os.environ["GETBLOCK_API_KEY"]),
    data="SELECT block_time, direction, base_token, quote_token FROM pumpswap_all_swaps LIMIT 5",
    timeout=60,
)
response.raise_for_status()
print(response.text)
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
			Database: "solana",
			Username: os.Getenv("GETBLOCK_USER_ID"),
			Password: os.Getenv("GETBLOCK_API_KEY"),
		},
	})
	defer db.Close()

	rows, err := db.Query("SELECT block_time, direction, base_token, quote_token FROM pumpswap_all_swaps LIMIT 5")
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
        .with_database("solana");

    let mut lines = client
        .query("SELECT block_time, direction, base_token, quote_token FROM pumpswap_all_swaps LIMIT 5")
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
```json
{"block_time":"2025-11-25 16:00:00","direction":"S","base_token":"J7Dx5yPbPD5e5BmrSvJyi4n4XRUR2kWqWYaxaSWdpump","quote_token":"So11111111111111111111111111111111111111112"}
{"block_time":"2025-11-25 16:00:00","direction":"B","base_token":"So11111111111111111111111111111111111111112","quote_token":"4qwCcewR4ewD6GDrJTiNJqezLDQYtGdxcLCT1AQNC11p"}
{"block_time":"2025-11-25 16:00:00","direction":"B","base_token":"So11111111111111111111111111111111111111112","quote_token":"D9Bm7bGSzPkqeKibu1adssv753Uy7j1NVbpYC3V9wfzp"}
{"block_time":"2025-11-25 16:00:00","direction":"B","base_token":"So11111111111111111111111111111111111111112","quote_token":"H7dbqcgExJY34kDrRqQrrXVYqodBreQz3mfDtHbV9NCf"}
{"block_time":"2025-11-25 16:00:00","direction":"B","base_token":"So11111111111111111111111111111111111111112","quote_token":"C1uSTAyKRb9Zt2ESsGYqQvB2y2armUwo2ctjr9AKWDS1"}

```
{% endcode %}

## Next steps

* [**API Reference**](./) — endpoint, authentication, and limits
* [**Querying Market SQL**](../querying-market-sql.md) — datasets, queries, and result formats
