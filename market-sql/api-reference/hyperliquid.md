---
description: >-
  Exchange and market activity for derivatives analytics, backtesting, and
  quantitative workflows.
---

# Hyperliquid - Market SQL

The **Hyperliquid** dataset provides order fills, perpetual-market activity, wallet positions, and aggregated order analytics.

<table data-search="false"><thead><tr><th>Property</th><th>Value</th></tr></thead><tbody><tr><td>Database name</td><td><code>hyperliquid</code></td></tr><tr><td>Tables</td><td>4</td></tr><tr><td>Access</td><td>Subscribed to separately</td></tr></tbody></table>

### Tables

<table data-search="false"><thead><tr><th>Table</th><th>Description</th></tr></thead><tbody><tr><td><code>agg_fulfilled_order</code></td><td>HyperLiquid fills aggregated by fulfilled order, including size, volume, PnL and wallet.</td></tr><tr><td><code>raw_node_fills_by_block</code></td><td>Raw HyperLiquid block fills with price, size, side, fee, PnL, TWAP, builder and liquidation context.</td></tr><tr><td><code>view_perpetual_wallet</code></td><td>Current perpetual-wallet analytics including realized PnL, volume, win counts and ROI distributions.</td></tr><tr><td><code>view_wallet_position</code></td><td>Current wallet positions by market with signed size and last update time.</td></tr></tbody></table>

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

{% tab title="Javascript" %}
{% code overflow="wrap" %}
```js
import { createClient } from '@clickhouse/client'

const client = createClient({
  host: 'https://market-sql.eu-central-1.getblock.io',
  username: process.env.GETBLOCK_USER_ID,
  password: process.env.GETBLOCK_API_KEY,
  database: 'hyperliquid'
})

const rows = await client.query({
  query: 'SELECT * FROM view_perpetual_wallet LIMIT 5',
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
    database='hyperliquid',
    secure=True,
)

print(client.query_df('SELECT * FROM view_perpetual_wallet LIMIT 5))
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
			Database: "hyperliquid",
			Username: os.Getenv("GETBLOCK_USER_ID"),
			Password: os.Getenv("GETBLOCK_API_KEY"),
		},
	})
	defer db.Close()

	rows, err := db.Query("SELECT * FROM view_perpetual_wallet LIMIT 5")
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
        .with_database("hyperliquid");

    let mut lines = client
        .query("SELECT * FROM view_perpetual_wallet LIMIT 5")
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
    "wallet_address": "0x34a7a6d7009195472ee528c920f83a9409063054",
    "total_pnl": 7.1958899999999995,
    "total_volume": 23778.658779999998,
    "cnt_unique_orders": 16,
    "last_utc_order_dt": "2026-01-02",
    "win_count": 6,
    "total_count": 32,
    "sum_order_roi": 0.9149486127193274,
    "sum_profitable_pnl": 38.124559999999995,
    "cnt_profitable_orders": 6,
    "sum_unprofitable_pnl": -30.92867,
    "cnt_unprofitable_orders": 6,
    "cnt_unique_coins": 1,
    "cnt_trade_days": 6,
    "roi_quantiles": [
        -0.6916557131849703,
        0,
        0,
        0,
        0,
        1.19254542919552
    ]
}
```
{% endcode %}

## Next steps

* [**API Reference**](./) — endpoint, authentication, and limits
* [**Querying Market SQL**](../querying-market-sql.md) — datasets, queries, and result formats
