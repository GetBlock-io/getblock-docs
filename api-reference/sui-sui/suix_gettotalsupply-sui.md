---
description: >-
  Example code for the suix_getTotalSupply JSON-RPC method. Complete guide on
  how to use suix_getTotalSupply JSON-RPC in GetBlock Web3 documentation.
---

# suix\_getTotalSupply - Sui

This method returns the total supply for a specified coin type on the SUI network. This method is useful for tokenomics analysis, market cap calculations, and monitoring token inflation or deflation over time. The supply value reflects the current total amount of the token in existence on-chain.

## Parameters

| Parameter  | Type   | Required | Description                                |
| ---------- | ------ | -------- | ------------------------------------------ |
| coin\_type | string | Yes      | The fully qualified type name for the coin |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "suix_getTotalSupply",
  "params": ["0x2::sui::SUI"]
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'suix_getTotalSupply',
  params: ['0x2::sui::SUI']
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
    "method": "suix_getTotalSupply",
    "params": ["0x2::sui::SUI"]
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
        "method": "suix_getTotalSupply",
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
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code title="response.json" %}
```json
{
  "jsonrpc": "2.0",
  "result": {
    "value": "10000000000000000000"
  },
  "id": "getblock.io"
}
```
{% endcode %}

## Response Parameters

| Parameter | Type   | Description                                   |
| --------- | ------ | --------------------------------------------- |
| value     | string | Total circulating supply in the smallest unit |

## Use Cases

* Calculate market capitalization for tokens
* Monitor token supply changes over time
* Analyze tokenomics and inflation rates
* Build supply tracking dashboards
* Verify token supply for audits

## Error Handling

| Error Code  | Description                             |
| ----------- | --------------------------------------- |
| -32602      | Invalid params - malformed coin type    |
| -32603      | Internal error - node processing issues |
| null result | Coin type does not have supply tracking |

## SDK Integration

{% tabs %}
{% tab title="Sui TypeScript SDK" %}
{% code title="typescript.ts" %}
```typescript
import { SuiClient } from '@mysten/sui/client';

const client = new SuiClient({ url: 'https://go.getblock.io/<ACCESS-TOKEN>/' });

const supply = await client.getTotalSupply({
  coinType: '0x2::sui::SUI',
});

console.log(supply);
```
{% endcode %}
{% endtab %}

{% tab title="Sui Rust SDK" %}
{% code title="rust.rs" %}
```rust
use sui_sdk::SuiClientBuilder;

#[tokio::main]
async fn main() -> Result<(), anyhow::Error> {
    let sui = SuiClientBuilder::default()
        .build("https://go.getblock.io/<ACCESS-TOKEN>/")
        .await?;
    
    let supply = sui.coin_read_api()
        .get_total_supply("0x2::sui::SUI".to_string())
        .await?;
    
    println!("{:?}", supply);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
