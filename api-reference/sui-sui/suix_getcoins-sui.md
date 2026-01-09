---
description: >-
  Example code for the suix_getCoins JSON-RPC method. Complete guide on how to
  use suix_getCoins JSON-RPC in GetBlock Web3 documentation.
---

# suix\_getCoins - Sui

This method returns all Coin objects of a specific type owned by an address on the SUI network. This filtered version of `suix_getAllCoins` is more efficient when you only need coins of a particular type, such as SUI for gas payments or a specific token for transfers. It supports pagination for addresses with many coin objects.

## Parameters

| Parameter  | Type       | Required | Description                                          |
| ---------- | ---------- | -------- | ---------------------------------------------------- |
| owner      | SuiAddress | Yes      | The owner's SUI address (0x prefixed hex string)     |
| coin\_type | string     | No       | Type name for the coin (defaults to `0x2::sui::SUI`) |
| cursor     | string     | No       | Optional paging cursor for pagination                |
| limit      | uint       | No       | Maximum number of items per page                     |

## Returns

| Field       | Type    | Description                                       |
| ----------- | ------- | ------------------------------------------------- |
| data        | array   | Array of Coin objects matching the specified type |
| nextCursor  | string  | Cursor for fetching the next page                 |
| hasNextPage | boolean | Indicates if more results are available           |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="curl" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "suix_getCoins",
  "params": [
    "0x94f1a597b4e8f709a396f7f6b1482bdcd65a673d111e49286c527fab7c2d0961",
    "0x2::sui::SUI",
    null,
    10
  ]
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="request.js" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'suix_getCoins',
  params: [
    '0x94f1a597b4e8f709a396f7f6b1482bdcd65a673d111e49286c527fab7c2d0961',
    '0x2::sui::SUI',
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
{% code title="request.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "suix_getCoins",
    "params": [
        "0x94f1a597b4e8f709a396f7f6b1482bdcd65a673d111e49286c527fab7c2d0961",
        "0x2::sui::SUI",
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
        "method": "suix_getCoins",
        "params": [
            "0x94f1a597b4e8f709a396f7f6b1482bdcd65a673d111e49286c527fab7c2d0961",
            "0x2::sui::SUI",
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
        "coinObjectId": "0xd62ca040aba24f862a763851c54908cd2a0ee7d709c11b93d4a2083747b76856",
        "version": "103626",
        "digest": "C9fdokK19BpDCgUgWsJv3cfd4LDyk7WGYBeGhFHbEL2Z",
        "balance": "200000000",
        "previousTransaction": "tw5DzJTfdxTn4f3rekFrhN7dQTUezBgsEhycDobTBLb"
      }
    ],
    "nextCursor": "0xd62ca040aba24f862a763851c54908cd2a0ee7d709c11b93d4a2083747b76856",
    "hasNextPage": true
  },
  "id": "getblock.io"
}
```
{% endcode %}

## Response Parameters

| Parameter           | Type    | Description                                |
| ------------------- | ------- | ------------------------------------------ |
| coinType            | string  | The fully qualified coin type              |
| coinObjectId        | string  | Unique identifier of the coin object       |
| version             | string  | Current version number of the object       |
| digest              | string  | Object digest hash                         |
| balance             | string  | Balance in the coin's smallest unit        |
| previousTransaction | string  | Last transaction that modified this object |
| nextCursor          | string  | Cursor for next page                       |
| hasNextPage         | boolean | True if more results exist                 |

## Use Cases

* Select SUI coins for gas payment in transactions
* Find specific token coins for transfer operations
* Implement efficient coin selection for payments
* List all coins of a custom token type
* Prepare inputs for Programmable Transaction Blocks

## Error Handling

| Error Code | Description                                     |
| ---------- | ----------------------------------------------- |
| -32602     | Invalid params - malformed address or coin type |
| -32603     | Internal error - node processing issues         |
| -32000     | Server error - RPC endpoint unavailable         |

## SDK Integration

{% tabs %}
{% tab title="Sui TypeScript SDK" %}
{% code title="sui.ts" %}
```typescript
import { SuiClient } from '@mysten/sui/client';

const client = new SuiClient({ url: 'https://go.getblock.io/<ACCESS-TOKEN>/' });

const coins = await client.getCoins({
  owner: '0x94f1a597b4e8f709a396f7f6b1482bdcd65a673d111e49286c527fab7c2d0961',
  coinType: '0x2::sui::SUI',
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
    let coins = sui.coin_read_api().get_coins(address, None, None, None).await?;
    
    println!("{:?}", coins);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
