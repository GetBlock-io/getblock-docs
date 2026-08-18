---
description: >-
  Example code for the eth_maxPriorityFeePerGas JSON RPC method. Сomplete guide
  on how to use eth_maxPriorityFeePerGas JSON RPC in GetBlock Web3
  documentation.
---

# eth\_maxPriorityFeePerGas - Ethereum

This method returns a node-suggested priority fee (tip) for inclusion in the next block, in wei, hex-encoded. Introduced with EIP-1559, this value is added to the block's `baseFeePerGas` to compute the effective gas price for a type-2 transaction. Node implementations use different heuristics — typical values are 1-2 gwei on Ethereum mainnet under normal conditions.

## Parameters

This method takes no parameters.

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_maxPriorityFeePerGas",
    "params": [],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_maxPriorityFeePerGas',
    params: [],
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'eth_maxPriorityFeePerGas',
        'params': [],
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
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "eth_maxPriorityFeePerGas",
            "params": [],
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
    "result": "0x3b9aca00"
}
```

## Response Parameters

| Parameter | Type   | Description                                |
| --------- | ------ | ------------------------------------------ |
| `jsonrpc` | string | JSON-RPC protocol version ("2.0")          |
| `id`      | string | Request identifier matching the request    |
| `result`  | string | Hex-encoded priority fee suggestion in wei |

## Use Cases

* **EIP-1559 Transaction Pricing**: Set `maxPriorityFeePerGas` on type-2 transactions to bid for inclusion in the next block
* **Wallet Fee Estimation**: Combine with the current base fee to produce a user-facing 'total fee' estimate
* **Congestion Detection**: Track priority fee over time to detect network congestion spikes
* **MEV-Aware Bidding**: Automated systems bidding for block-space use this as a floor and add competitive premium

## Error Handling

| Error Code | Message        | Description                                           |
| ---------- | -------------- | ----------------------------------------------------- |
| -32603     | Internal error | Node's priority fee oracle failed to produce a result |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const feeData = await provider.getFeeData();
console.log('Max priority fee (gwei):', ethers.formatUnits(feeData.maxPriorityFeePerGas ?? 0n, 'gwei'));
console.log('Max fee per gas (gwei):', ethers.formatUnits(feeData.maxFeePerGas ?? 0n, 'gwei'));
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http, formatGwei } from 'viem';
import { mainnet } from 'viem/chains';

const client = createPublicClient({
    chain: mainnet,
    transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/'),
});

const priorityFee = await client.estimateMaxPriorityFeePerGas();
console.log('Max priority fee (gwei):', formatGwei(priorityFee));
```
{% endcode %}
{% endtab %}
{% endtabs %}
