---
description: >-
  Market, order, and trading data for prediction-market research and strategy
  analysis.
---

# Polymarket - Market SQL

The **Polymarket** dataset provides decoded order fills from Polygon, together with event and market metadata such as titles, categories, outcomes, status, and resolution.

<table data-search="false"><thead><tr><th>Property</th><th>Value</th></tr></thead><tbody><tr><td>Database name</td><td><code>polymarket</code></td></tr><tr><td>Tables</td><td>3</td></tr><tr><td>Access</td><td>Subscribed to separately</td></tr></tbody></table>

### Tables

<table data-search="false"><thead><tr><th>Table</th><th>Description</th></tr></thead><tbody><tr><td><code>polymarket_order_filled_v3</code></td><td>Decoded Polygon maker and taker fill legs with outcome tokens, USDC amounts, fees and block context.</td></tr><tr><td><code>raw_event_meta</code></td><td>Polymarket event titles, slugs, dates, categories, status, liquidity and resolution metadata.</td></tr><tr><td><code>raw_market_meta</code></td><td>Polymarket questions, outcomes, token identifiers, trading status and resolution metadata.</td></tr></tbody></table>

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

{% tab title="Javascript" %}
{% code overflow="wrap" %}
```js
import { createClient } from '@clickhouse/client'

const client = createClient({
  host: 'https://market-sql.eu-central-1.getblock.io',
  username: process.env.GETBLOCK_USER_ID,
  password: process.env.GETBLOCK_API_KEY,
  database: 'polymarket'
})

const rows = await client.query({
  query: 'SELECT * FROM raw_market_meta LIMIT 5',
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
    database='polymarket',
    secure=True,
)

print(client.query_df('SELECT * FROM raw_market_meta LIMIT 5'))
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
			Database: "polymarket",
			Username: os.Getenv("GETBLOCK_USER_ID"),
			Password: os.Getenv("GETBLOCK_API_KEY"),
		},
	})
	defer db.Close()

	rows, err := db.Query("SELECT * FROM raw_market_meta LIMIT 5")
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
        .with_database("polymarket");

    let mut lines = client
        .query("SELECT * FROM raw_market_meta LIMIT 5")
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
    "market_id": "0x0923f4cd56ffdd032deca9984f9779303b58c6ef9317c0c6490acccb477ba150",
    "question": "Spurs vs. Knicks",
    "condition_id": "0x0923f4cd56ffdd032deca9984f9779303b58c6ef9317c0c6490acccb477ba150",
    "slug": "nba-sas-nyk-2026-06-08",
    "twitter_card_image": null,
    "resolution_source": null,
    "market_end_dttm": "2026-06-09 00:00:00.000",
    "market_start_dttm": null,
    "category": null,
    "amm_type": null,
    "sponsor_name": null,
    "sponsor_image": null,
    "x_axis_value": null,
    "y_axis_value": null,
    "denomination_token": null,
    "fee": null,
    "lower_bound": null,
    "upper_bound": null,
    "description": "In the upcoming NBA game, scheduled for June 8 at 8:30PM ET:\nIf the Spurs win, the market will resolve to \"Spurs\".\nIf the Knicks win, the market will resolve to \"Knicks\".\nIf the game is postponed, this market will remain open until the game has been completed.\nIf the game is canceled entirely, with no make-up game, this market will resolve 50-50.\nThe result will be determined based on the final score including any overtime periods.",
    "outcome": "Spurs",
    "outcome_price": 1,
    "clob_token_id": "103146822800955228694960469584465618974402104793575818808187047895725122181216",
    "volume": null,
    "volume_num": null,
    "is_active": true,
    "market_type": null,
    "format_type": null,
    "lower_bound_dttm": null,
    "upper_bound_dttm": null,
    "is_closed": true,
    "market_maker_address": null,
    "created_by": null,
    "updated_by": null,
    "created_dttm": null,
    "updated_dttm": null,
    "closed_dttm": null,
    "is_wide_format": null,
    "is_new": null,
    "mailchimp_tag": null,
    "is_featured": null,
    "is_archived": false,
    "resolved_by": null,
    "is_restricted": null,
    "market_group": null,
    "group_item_title": null,
    "group_item_threshold": null,
    "question_id": "0x33e91f51acb29ae791223524070fdbfc57bce64d39dbd3367b971f4ee37b39b7",
    "uma_end_dttm": null,
    "is_enable_order_book": false,
    "order_price_min_tick_size": 0.001,
    "order_min_size": 5,
    "uma_resolution_status": null,
    "curation_order": null,
    "liquidity_num": null,
    "end_date_iso_dt_str": "2026-06-09T00:00:00Z",
    "start_date_iso_dt_str": null,
    "uma_end_date_iso_dt_str": null,
    "is_having_reviewed_dates": null,
    "is_ready_for_cron": null,
    "is_comments_enabled": null,
    "volume_24hr": null,
    "volume_1wk": null,
    "volume_1mo": null,
    "volume_1yr": null,
    "game_start_time": "2026-06-09T00:30:00Z",
    "seconds_delay": 0,
    "disqus_thread": null,
    "short_outcome": null,
    "team_a_id": null,
    "team_b_id": null,
    "uma_bond": null,
    "uma_reward": null,
    "is_fpmm_live": null,
    "volume_24hr_amm": null,
    "volume_1wk_amm": null,
    "volume_1mo_amm": null,
    "volume_1yr_amm": null,
    "volume_24hr_clob": null,
    "volume_1wk_clob": null,
    "volume_1mo_clob": null,
    "volume_1yr_clob": null,
    "volume_amm": null,
    "volume_clob": null,
    "liquidity": null,
    "liquidity_amm": null,
    "liquidity_clob": null,
    "maker_base_fee": 1000,
    "taker_base_fee": 1000,
    "custom_liveness": null,
    "is_accepting_orders": false,
    "is_notifications_enabled": true,
    "score": null,
    "image_optimized": null,
    "icon_optimized": null,
    "event": null,
    "tag": "\"Sports\"",
    "category_meta": null,
    "creator": null,
    "is_ready": null,
    "is_funded": null,
    "past_slugs": null,
    "ready_timestamp": null,
    "funded_timestamp": null,
    "accepting_orders_timestamp": "2026-06-02 04:01:46.000",
    "competitive": null,
    "rewards_min_size": 50,
    "rewards_max_spread": 4.5,
    "spread": null,
    "is_automatically_resolved": null,
    "one_day_price_change": null,
    "one_hour_price_change": null,
    "one_week_price_change": null,
    "one_month_price_change": null,
    "one_year_price_change": null,
    "last_trade_price": null,
    "best_bid": null,
    "best_ask": null,
    "is_automatically_active": null,
    "is_clear_book_on_start": null,
    "chart_color": null,
    "series_color": null,
    "is_showing_gmp_series": null,
    "is_showing_gmp_outcome": null,
    "is_manual_activation": null,
    "is_neg_risk_other": null,
    "game_id": null,
    "group_item_range": null,
    "sports_market_type": null,
    "line": null,
    "uma_resolution_statuses": null,
    "is_pending_deployment": null,
    "is_deploying": null,
    "deploying_timestamp": null,
    "scheduled_deployment_timestamp": null,
    "is_rfq_enabled": null,
    "event_start_time": null,
    "image": "https://polymarket-upload.s3.us-east-2.amazonaws.com/super+cool+basketball+in+red+and+blue+wow.png",
    "icon": "https://polymarket-upload.s3.us-east-2.amazonaws.com/super+cool+basketball+in+red+and+blue+wow.png",
    "raw_json": "{\"enable_order_book\": false, \"active\": true, \"closed\": true, \"archived\": false, \"accepting_orders\": false, \"accepting_order_timestamp\": \"2026-06-02T04:01:46Z\", \"minimum_order_size\": 5, \"minimum_tick_size\": 0.001, \"condition_id\": \"0x0923f4c \"NBA\", \"Games\"]}",
    "inserted_at": "2026-07-10 21:21:05"
}
{
    "market_id": "0x0923f4cd56ffdd032deca9984f9779303b58c6ef9317c0c6490acccb477ba150",
    "question": "Spurs vs. Knicks",
    "condition_id": "0x0923f4cd56ffdd032deca9984f9779303b58c6ef9317c0c6490acccb477ba150",
    "slug": "nba-sas-nyk-2026-06-08",
    "twitter_card_image": null,
    "resolution_source": null,
    "market_end_dttm": "2026-06-09 00:00:00.000",
    "market_start_dttm": null,
    "category": null,
    "amm_type": null,
    "sponsor_name": null,
    "sponsor_image": null,
    "x_axis_value": null,
    "y_axis_value": null,
    "denomination_token": null,
    "fee": null,
    "lower_bound": null,
    "upper_bound": null,
    "description": "In the upcoming NBA game, scheduled for June 8 at 8:30PM ET:\nIf the Spurs win, the market will resolve to \"Spurs\".\nIf the Knicks win, the market will resolve to \"Knicks\".\nIf the game is postponed, this market will remain open until the game has been completed.\nIf the game is canceled entirely, with no make-up game, this market will resolve 50-50.\nThe result will be determined based on the final score including any overtime periods.",
    "outcome": "Knicks",
    "outcome_price": 0,
    "clob_token_id": "98459181579236923586844159348634682860151195540312195350235235649190114114052",
    "volume": null,
    "volume_num": null,
    "is_active": true,
    "market_type": null,
    "format_type": null,
    "lower_bound_dttm": null,
    "upper_bound_dttm": null,
    "is_closed": true,
    "market_maker_address": null,
    "created_by": null,
    "updated_by": null,
    "created_dttm": null,
    "updated_dttm": null,
    "closed_dttm": null,
    "is_wide_format": null,
    "is_new": null,
    "mailchimp_tag": null,
    "is_featured": null,
    "is_archived": false,
    "resolved_by": null,
    "is_restricted": null,
    "market_group": null,
    "group_item_title": null,
    "group_item_threshold": null,
    "question_id": "0x33e91f51acb29ae791223524070fdbfc57bce64d39dbd3367b971f4ee37b39b7",
    "uma_end_dttm": null,
    "is_enable_order_book": false,
    "order_price_min_tick_size": 0.001,
    "order_min_size": 5,
    "uma_resolution_status": null,
    "curation_order": null,
    "liquidity_num": null,
    "end_date_iso_dt_str": "2026-06-09T00:00:00Z",
    "start_date_iso_dt_str": null,
    "uma_end_date_iso_dt_str": null,
    "is_having_reviewed_dates": null,
    "is_ready_for_cron": null,
    "is_comments_enabled": null,
    "volume_24hr": null,
    "volume_1wk": null,
    "volume_1mo": null,
    "volume_1yr": null,
    "game_start_time": "2026-06-09T00:30:00Z",
    "seconds_delay": 0,
    "disqus_thread": null,
    "short_outcome": null,
    "team_a_id": null,
    "team_b_id": null,
    "uma_bond": null,
    "uma_reward": null,
    "is_fpmm_live": null,
    "volume_24hr_amm": null,
    "volume_1wk_amm": null,
    "volume_1mo_amm": null,
    "volume_1yr_amm": null,
    "volume_24hr_clob": null,
    "volume_1wk_clob": null,
    "volume_1mo_clob": null,
    "volume_1yr_clob": null,
    "volume_amm": null,
    "volume_clob": null,
    "liquidity": null,
    "liquidity_amm": null,
    "liquidity_clob": null,
    "maker_base_fee": 1000,
    "taker_base_fee": 1000,
    "custom_liveness": null,
    "is_accepting_orders": false,
    "is_notifications_enabled": true,
    "score": null,
    "image_optimized": null,
    "icon_optimized": null,
    "event": null,
    "tag": "\"Sports\"",
    "category_meta": null,
    "creator": null,
    "is_ready": null,
    "is_funded": null,
    "past_slugs": null,
    "ready_timestamp": null,
    "funded_timestamp": null,
    "accepting_orders_timestamp": "2026-06-02 04:01:46.000",
    "competitive": null,
    "rewards_min_size": 50,
    "rewards_max_spread": 4.5,
    "spread": null,
    "is_automatically_resolved": null,
    "one_day_price_change": null,
    "one_hour_price_change": null,
    "one_week_price_change": null,
    "one_month_price_change": null,
    "one_year_price_change": null,
    "last_trade_price": null,
    "best_bid": null,
    "best_ask": null,
    "is_automatically_active": null,
    "is_clear_book_on_start": null,
    "chart_color": null,
    "series_color": null,
    "is_showing_gmp_series": null,
    "is_showing_gmp_outcome": null,
    "is_manual_activation": null,
    "is_neg_risk_other": null,
    "game_id": null,
    "group_item_range": null,
    "sports_market_type": null,
    "line": null,
    "uma_resolution_statuses": null,
    "is_pending_deployment": null,
    "is_deploying": null,
    "deploying_timestamp": null,
    "scheduled_deployment_timestamp": null,
    "is_rfq_enabled": null,
    "event_start_time": null,
    "image": "https://polymarket-upload.s3.us-east-2.amazonaws.com/super+cool+basketball+in+red+and+blue+wow.png",
    "icon": "https://polymarket-upload.s3.us-east-2.amazonaws.com/super+cool+basketball+in+red+and+blue+wow.png",
    "raw_json": "{\"enable_order_book\": false, \"active\": true, \"closed\": true, \"archived\": false, \"accepting_orders\": false, \"accepting_order_timestamp\": \"2026-06-02T04:01:46Z\", \"minimum_order_size\": 5, \"minimum_tick_size\": 0.001, \"condition_id\": \"0x0923f4cd56ffdd, \"Basketball\", \"NBA\", \"Games\"]}",
    "inserted_at": "2026-07-10 21:21:05"
}
```
{% endcode %}

## Next steps

* [**API Reference**](./) — endpoint, authentication, and limits
* [**Querying Market SQL**](../querying-market-sql.md) — datasets, queries, and result formats
