---
description: >-
  Example code for the eth_syncing JSON RPC method. Сomplete guide on how to use
  eth_syncing GetBlock JSON RPC in GetBlock Web3 documentation.
---

# eth\_syncing - HyperEVM

This method returns an object with data about the sync status or `false` if not syncing.

## Parameters

* None

## Request

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_syncing",
    "params": [],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="JavaScript (axios)" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_syncing',
    params: [],
    id: 'getblock.io'
});

console.log('Syncing:', response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="Python (requests)" %}
```python
import requests

response = requests.post('https://go.getblock.io/<ACCESS-TOKEN>/', json={
    'jsonrpc': '2.0',
    'method': 'eth_syncing',
    'params': [],
    'id': 'getblock.io'
})

result = response.json()['result']
if result == False:
    print('Node is fully synced')
else:
    print(f'Syncing: {result}')
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="Rust (reqwest) / async" %}
```rust
use reqwest::header;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = reqwest::Client::new();
    
    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header(header::CONTENT_TYPE, "application/json")
        .body(r#"{
            "jsonrpc": "2.0",
            "method": "eth_syncing",
            "params": [],
            "id": "getblock.io"
        }"#)
        .send()
        .await?;
    
    println!("{}", response.text().await?);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

{% code title="Example response" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": false
}
```
{% endcode %}

## Response Parameters

| Field  | Type             | Description                                    |
| ------ | ---------------- | ---------------------------------------------- |
| result | boolean / object | `false` if not syncing, or sync status object. |

### Sync Status Object (if syncing)

| Field         | Type   | Description                        |
| ------------- | ------ | ---------------------------------- |
| startingBlock | string | Block at which sync started (hex). |
| currentBlock  | string | Current block being synced (hex).  |
| highestBlock  | string | Estimated highest block (hex).     |

## Use Case

The `eth_syncing` method is essential for:

* Node health monitoring
* Sync status verification
* Application readiness checks
* Infrastructure monitoring

## Error Handling

| Error Code | Message        | Cause       |
| ---------- | -------------- | ----------- |
| -32603     | Internal error | Node issue. |

### Web3 Integration

{% tabs %}
{% tab title="Ether.js" %}
```javascript
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.io/<ACCESS_TOKEN>";
const provider = new ethers.JsonRpcProvider(RPC_URL);

async function Call() {
  try {
    // Call the method — this will resolve to a number (promise resolves to number
    const result = await provider.send("eth_syncing", []);
    console.log("The result:", result);
    return result;
  } catch (error) {
    console.error("The error:", error);
    throw error;
  }
}
```
{% endtab %}

{% tab title="Viem" %}
```jsx
import { createPublicClient, http } from "viem";
import { hyperEvm } from "viem/chains";

// Create Viem client with GetBlock
const client = createPublicClient({
  chain: hyperEvm,
  transport: http(import.meta.env.HYPER_ACCESS_TOKEN),
});

// Using the method through Viem
async function Call() {
  try {
    // Method-specific Viem implementation
    const result = await client.request({
      method: "eth_syncing",
      params: [],
    });
    return result;
  } catch (error) {
    console.error("Viem Error:", error);
    throw error;
  }
}
```
{% endtab %}
{% endtabs %}
