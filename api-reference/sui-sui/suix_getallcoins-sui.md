---
description: >-
  Example code for the suix_getAllCoins JSON-RPC method. Complete guide on how
  to use suix_getAllCoins JSON-RPC in GetBlock Web3 documentation.
---

# suix\_getAllCoins - Sui

This method returns all Coin objects owned by an address on the SUI network with pagination support. Unlike `suix_getAllBalances` which returns aggregated totals, this method returns individual coin objects with their unique IDs, versions, and balances. This is essential for transaction building, coin selection algorithms, and detailed asset management.

## Parameters

| Parameter | Type       | Required | Description                                               |
| --------- | ---------- | -------- | --------------------------------------------------------- |
| owner     | SuiAddress | Yes      | The owner's SUI address (0x prefixed hex string)          |
| cursor    | string     | No       | Optional paging cursor for pagination                     |
| limit     | uint       | No       | Maximum number of items per page (default varies by node) |

## Returns

| Field       | Type    | Description                                     |
| ----------- | ------- | ----------------------------------------------- |
| data        | array   | Array of Coin objects with detailed information |
| nextCursor  | string  | Cursor for fetching the next page of results    |
| hasNextPage | boolean | Indicates if more results are available         |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "suix_getAllCoins",
  "params": [
    "0x94f1a597b4e8f709a396f7f6b1482bdcd65a673d111e49286c527fab7c2d0961",
    null,
    10
  ]
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'suix_getAllCoins',
  params: [
    '0x94f1a597b4e8f709a396f7f6b1482bdcd65a673d111e49286c527fab7c2d0961',
    null,
    10
  ]
};

axios.post(url, payload, {
  headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log(response.data))
.catch(error => console.error(error));
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "suix_getAllCoins",
    "params": [
        "0x94f1a597b4e8f709a396f7f6b1482bdcd65a673d111e49286c527fab7c2d0961",
        None,
        10
    ]
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="main.rs" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let payload = json!({
        "jsonrpc": "2.0",
        "id": "getblock.io",
        "method": "suix_getAllCoins",
        "params": [
            "0x94f1a597b4e8f709a396f7f6b1482bdcd65a673d111e49286c527fab7c2d0961",
            null,
            10
        ]
    });

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&payload)
        .send()
        .await?;

    let result: serde_json::Value = response.json().await?;
    println!("{:#?}", result);
    
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
  "jsonrpc": "2.0",
  "result": {
    "data": [
      {
        "coinType": "0x2::sui::SUI",
        "coinObjectId": "0x861c5e055605b2bb1199faf653a8771e448930bc95a0369fad43a9870a2e5878",
        "version": "103626",
        "digest": "Ao1QyN9UTmYzb2ead3D5xhSBk7TvACRvmnJW8gRbwP99",
        "balance": "200000000",
        "previousTransaction": "7dp5WtTmtGp83EXYYFMzjBJRFeSgR67AzqMETLrfgeFx"
      }
    ],
    "nextCursor": "0x861c5e055605b2bb1199faf653a8771e448930bc95a0369fad43a9870a2e5878",
    "hasNextPage": true
  },
  "id": "getblock.io"
}
```
{% endcode %}

## Response Parameters

| Parameter           | Type    | Description                                              |
| ------------------- | ------- | -------------------------------------------------------- |
| coinType            | string  | The fully qualified coin type                            |
| coinObjectId        | string  | Unique identifier of the coin object                     |
| version             | string  | Current version number of the object                     |
| digest              | string  | Object digest hash for verification                      |
| balance             | string  | Balance held in this specific coin object                |
| previousTransaction | string  | Digest of the transaction that last modified this object |
| nextCursor          | string  | Cursor for pagination to fetch next page                 |
| hasNextPage         | boolean | True if more results exist beyond this page              |

## Use Cases

* Select specific coin objects for transaction inputs
* Implement coin selection algorithms for optimal gas usage
* Display detailed coin inventory in wallets
* Merge multiple small coin objects into larger ones
* Track individual coin object history

## Error Handling

| Error Code | Description                                  |
| ---------- | -------------------------------------------- |
| -32602     | Invalid params - malformed address or cursor |
| -32603     | Internal error - node processing issues      |
| -32000     | Server error - RPC endpoint unavailable      |

## SDK Integration

{% tabs %}
{% tab title="Sui TypeScript SDK" %}
{% code title="example.ts" %}
```typescript
import { SuiClient } from '@mysten/sui/client';

const client = new SuiClient({ url: 'https://go.getblock.io/<ACCESS-TOKEN>/' });

const coins = await client.getAllCoins({
  owner: '0x94f1a597b4e8f709a396f7f6b1482bdcd65a673d111e49286c527fab7c2d0961',
});

console.log(coins);
```
{% endcode %}
{% endtab %}

{% tab title="Sui Rust SDK" %}
{% code title="main.rs" overflow="wrap" %}
```rust
use sui_sdk::SuiClientBuilder;

#[tokio::main]
async fn main() -> Result<(), anyhow::Error> {
    let sui = SuiClientBuilder::default()
        .build("https://go.getblock.io/<ACCESS-TOKEN>/")
        .await?;
    
    let address = "0x94f1a597b4e8f709a396f7f6b1482bdcd65a673d111e49286c527fab7c2d0961".parse()?;
    let coins = sui.coin_read_api().get_all_coins(address, None, None).await?;
    
    println!("{:?}", coins);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
