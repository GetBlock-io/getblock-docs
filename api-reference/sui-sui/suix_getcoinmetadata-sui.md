---
description: >-
  Example code for the suix_getCoinMetadata JSON-RPC method. Complete guide on
  how to use suix_getCoinMetadata JSON-RPC in GetBlock Web3 documentation.
---

# suix\_getCoinMetadata - Sui

This method returns metadata for a specified coin type on the SUI network, including its symbol, decimals, name, description, and icon URL. This information is essential for displaying tokens correctly in wallet interfaces, DEXs, and portfolio trackers. The metadata is stored on-chain and reflects the token's official configuration.

## Parameters

| Parameter  | Type   | Required | Description                                                        |
| ---------- | ------ | -------- | ------------------------------------------------------------------ |
| coin\_type | string | Yes      | The fully qualified type name for the coin (e.g., `0x2::sui::SUI`) |

## Returns

| Field       | Type     | Description                                     |
| ----------- | -------- | ----------------------------------------------- |
| decimals    | uint8    | Number of decimal places for display formatting |
| name        | string   | Human-readable name of the coin                 |
| symbol      | string   | Trading symbol (e.g., SUI, USDC)                |
| description | string   | Description of the coin and its purpose         |
| iconUrl     | string   | URL to the coin's icon image (may be null)      |
| id          | ObjectID | The CoinMetadata object ID on-chain             |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "suix_getCoinMetadata",
  "params": [
    "0x2::sui::SUI"
  ]
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'suix_getCoinMetadata',
  params: ['0x2::sui::SUI']
};

axios.post(url, payload, {
  headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log(response.data))
.catch(error => console.error(error));
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "suix_getCoinMetadata",
    "params": ["0x2::sui::SUI"]
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endtab %}

{% tab title="Rust" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let payload = json!({
        "jsonrpc": "2.0",
        "id": "getblock.io",
        "method": "suix_getCoinMetadata",
        "params": ["0x2::sui::SUI"]
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
{% endtab %}
{% endtabs %}

## Response Example

```json
{
  "jsonrpc": "2.0",
  "result": {
    "decimals": 9,
    "name": "Sui",
    "symbol": "SUI",
    "description": "The native token of the Sui network.",
    "iconUrl": null,
    "id": "0x587c29de216efd4219573e08a1f6964d4fa7cb714518c2c8a0f29abfa264327d"
  },
  "id": "getblock.io"
}
```

## Response Parameters

| Parameter   | Type    | Description                                                      |
| ----------- | ------- | ---------------------------------------------------------------- |
| decimals    | integer | Number of decimal places (SUI uses 9, meaning 1 SUI = 10^9 MIST) |
| name        | string  | Full name of the token                                           |
| symbol      | string  | Short trading symbol                                             |
| description | string  | Token description                                                |
| iconUrl     | string  | URL to token icon (null if not set)                              |
| id          | string  | On-chain CoinMetadata object ID                                  |

## Use Cases

* Display correct token symbols and names in wallets
* Format balances with proper decimal places
* Show token icons in user interfaces
* Validate token information for listings
* Build token registries and databases

## Error Handling

| Error Code  | Description                                 |
| ----------- | ------------------------------------------- |
| -32602      | Invalid params - malformed coin type        |
| -32603      | Internal error - node processing issues     |
| null result | Coin type does not have registered metadata |

## SDK Integration

{% tabs %}
{% tab title="Sui TypeScript SDK" %}
```typescript
import { SuiClient } from '@mysten/sui/client';

const client = new SuiClient({ url: 'https://go.getblock.io/<ACCESS-TOKEN>/' });

const metadata = await client.getCoinMetadata({
  coinType: '0x2::sui::SUI',
});

console.log(metadata);
```
{% endtab %}

{% tab title="Sui Rust SDK" %}
```rust
use sui_sdk::SuiClientBuilder;

#[tokio::main]
async fn main() -> Result<(), anyhow::Error> {
    let sui = SuiClientBuilder::default()
        .build("https://go.getblock.io/<ACCESS-TOKEN>/")
        .await?;
    
    let metadata = sui.coin_read_api()
        .get_coin_metadata("0x2::sui::SUI".to_string())
        .await?;
    
    println!("{:?}", metadata);
    Ok(())
}
```
{% endtab %}
{% endtabs %}
