---
description: >-
  Example code for the eth_getSystemTxsByBlockNumber JSON RPC method. Сomplete
  guide on how to use eth_getSystemTxsByBlockNumber GetBlock JSON RPC in
  GetBlock Web3 documentation.
---

# eth\_getSystemTxsByBlockNumber - HyperEVM

This method gets the system transactions that originate from HyperCore for a given block number.&#x20;

## Parameters

| Parameter   | Type   | Required | Description                     |
| ----------- | ------ | -------- | ------------------------------- |
| blockNumber | string | Yes      | Block number (hex) or "latest". |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getSystemTxsByBlockNumber",
    "params": ["latest"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="JavaScript" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_getSystemTxsByBlockNumber',
    params: ['latest'],
    id: 'getblock.io'
});

console.log('System Transactions:', response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="Python" %}
```python
import requests

response = requests.post('https://go.getblock.io/<ACCESS-TOKEN>/', json={
    'jsonrpc': '2.0',
    'method': 'eth_getSystemTxsByBlockNumber',
    'params': ['latest'],
    'id': 'getblock.io'
})

print(f'System Transactions: {response.json()["result"]}')
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="Rust" %}
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
            "method": "eth_getSystemTxsByBlockNumber",
            "params": ["latest"],
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

{% code title="Response (example)" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": [
        {
            "hash": "0x...",
            "blockNumber": "0x1b4",
            "from": "0x0000000000000000000000000000000000000000",
            "to": "0x...",
            "value": "0x0",
            "input": "0x...",
            "type": "system"
        }
    ]
}
```
{% endcode %}

## Response Parameters

{% tabs %}
{% tab title="Top-level fields" %}
| Field  | Type  | Description                                         |
| ------ | ----- | --------------------------------------------------- |
| result | array | Array of system transaction objects from HyperCore. |
{% endtab %}

{% tab title="Fields of a system transaction object" %}
| Field       | Type   | Description                                         |
| ----------- | ------ | --------------------------------------------------- |
| hash        | string | Transaction hash.                                   |
| blockNumber | string | Block number (hex).                                 |
| from        | string | Origin address (often zero address for system txs). |
| to          | string | Target address.                                     |
| value       | string | Value transferred (hex).                            |
| input       | string | Transaction data (hex).                             |
| type        | string | Transaction type ("system").                        |
{% endtab %}
{% endtabs %}

## Use Case

The `eth_getSystemTxsByBlockNumber` method is essential for:

* Sequential processing of system transactions
* Real-time monitoring of HyperCore interactions
* Indexing cross-layer operations by block
* Building comprehensive block explorers
* Tracking HyperCore state changes on HyperEVM

## Error Handling

| Error Code | Message        | Cause                        |
| ---------- | -------------- | ---------------------------- |
| -32602     | Invalid params | Invalid block number format. |
| -32603     | Internal error | Block not found.             |

### Web3 Integration

{% tabs %}
{% tab title="Ether.js" %}
```javascript
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.io/<ACCESS_TOKEN>";
const provider = new ethers.JsonRpcProvider(RPC_URL);

async function Call() {
  try {
    // Call the method — this will resolve to a number (promise resolves to number)
    const result = await provider.send("eth_getSystemTxsByBlockNumber", ["latest"]);
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
      method: "eth_getSystemTxsByBlockNumber",
      params: ["latest"],
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
