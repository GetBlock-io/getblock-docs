---
description: >-
  Example code for the linea_estimateGas JSON-RPC method. Complete guide on how
  to use linea_estimateGas JSON-RPC in GetBlock Web3 documentation.
---

# linea\_estimateGas - Linea

This method returns a Linea-specific gas estimate for a transaction, including the recommended gas limit, the base fee per gas, and the priority fee per gas. It takes the same inputs as eth\_estimateGas but accounts for Linea's Layer 1 data-submission cost, so it produces a more accurate price for Linea transactions and helps avoid rejection for exceeding module limits.

## Parameters

| Parameter   | Type   | Required | Description             |
| ----------- | ------ | -------- | ----------------------- |
| transaction | object | Yes      | Transaction call object |

### Transaction Object

| Field    | Type   | Required | Description                           |
| -------- | ------ | -------- | ------------------------------------- |
| from     | string | No       | 20-byte sender address                |
| to       | string | Yes      | 20-byte recipient or contract address |
| gas      | string | No       | Gas limit for the transaction (hex)   |
| gasPrice | string | No       | Gas price in wei (hex)                |
| value    | string | No       | Value to send in wei (hex)            |
| data     | string | No       | Encoded function call data            |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "linea_estimateGas",
    "params": [{"from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "to": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "value": "0x16345785d8a0000"}],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'linea_estimateGas',
    params: [{"from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "to": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "value": "0x16345785d8a0000"}],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests

response = requests.post(
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'linea_estimateGas',
        'params': [{"from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "to": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "value": "0x16345785d8a0000"}],
        'id': 'getblock.io'
    }
)

print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="example.rs" %}
```rust
use reqwest::Client;
use serde_json::{json, Value};

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "linea_estimateGas",
            "params": [{"from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "to": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "value": "0x16345785d8a0000"}],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;

    println!("Result: {}", response["result"]);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "baseFeePerGas": "0x7",
        "gasLimit": "0xcf08",
        "priorityFeePerGas": "0x43a82a4"
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                                    |
| --------- | ------ | -------------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                              |
| id        | string | Request identifier matching the request                        |
| result    | object | Object with the recommended gas limit and Linea fee components |

### Result Object

| Field             | Type   | Description                                                   |
| ----------------- | ------ | ------------------------------------------------------------- |
| baseFeePerGas     | string | Base fee per gas in wei, hex-encoded                          |
| gasLimit          | string | Recommended gas limit for the transaction, hex-encoded        |
| priorityFeePerGas | string | Priority fee per gas in wei, including the L1 submission cost |

## Use Cases

* **Accurate Pricing**: Price a Linea transaction with L1 costs included
* **Rejection Avoidance**: Set a gas price high enough for sequencer inclusion
* **Wallet Fee UX**: Show a Linea-accurate fee before signing
* **Batch Budgeting**: Size operations against Linea's module limits

## Error Handling

| Error Code | Message            | Description                                                    |
| ---------- | ------------------ | -------------------------------------------------------------- |
| -32602     | Invalid params     | The transaction object is missing required fields or malformed |
| -32000     | Execution error    | The transaction cannot be priced against the current state     |
| 3          | Execution reverted | The transaction would revert on execution                      |
| -32603     | Internal error     | The node failed to produce an estimate                         |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const tx = { from: '0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045', to: '0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045', value: '0x16345785d8a0000' };
const estimate = await provider.send('linea_estimateGas', [tx]);
console.log(estimate);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { linea } from 'viem/chains';

const client = createPublicClient({ chain: linea, transport: http('https://go.getblock.io/<ACCESS-TOKEN>/') });

const estimate = await client.request({
    method: 'linea_estimateGas',
    params: [{ from: '0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045', to: '0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045', value: '0x16345785d8a0000' }]
});
console.log(estimate);
```
{% endcode %}
{% endtab %}
{% endtabs %}
