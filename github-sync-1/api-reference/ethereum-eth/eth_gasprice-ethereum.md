---
description: >-
  Example code for the eth_gasPrice JSON RPC method. Сomplete guide on how to
  use eth_gasPrice JSON RPC in GetBlock Web3 documentation.
---

# eth\_gasPrice - Ethereum

This method returns the current gas price estimate in wei, hex-encoded. On post-London Ethereum (EIP-1559 and later), this returns a legacy-transaction gas price that includes both the base fee and a suggested priority tip — kept for backward compatibility with pre-1559 tooling. Modern applications should prefer `eth_maxPriorityFeePerGas` combined with the block's `baseFeePerGas` for EIP-1559 pricing.

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
    "method": "eth_gasPrice",
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
    method: 'eth_gasPrice',
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
        'method': 'eth_gasPrice',
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
            "method": "eth_gasPrice",
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
    "result": "0x9502f900"
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| `jsonrpc` | string | JSON-RPC protocol version ("2.0")       |
| `id`      | string | Request identifier matching the request |
| `result`  | string | Hex-encoded gas price estimate in wei   |

## Use Cases

* **Legacy Transaction Pricing**: Set `gasPrice` on type-0 legacy transactions from older tooling
* **Wallet Fee Estimation**: Baseline for a UI fee estimator (though EIP-1559 pricing gives finer control)
* **Cost Projection**: Estimate the wei cost of an operation before sending

## Error Handling

| Error Code | Message        | Description                                     |
| ---------- | -------------- | ----------------------------------------------- |
| -32603     | Internal error | Node's gas estimator failed to produce a result |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const feeData = await provider.getFeeData();
console.log('Gas price (wei):', feeData.gasPrice?.toString());
console.log('Gas price (gwei):', ethers.formatUnits(feeData.gasPrice ?? 0n, 'gwei'));
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

const gasPrice = await client.getGasPrice();
console.log('Gas price (wei):', gasPrice);
console.log('Gas price (gwei):', formatGwei(gasPrice));
```
{% endcode %}
{% endtab %}
{% endtabs %}
