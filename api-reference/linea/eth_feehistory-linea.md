---
description: >-
  Example code for the eth_feeHistory JSON-RPC method. Complete guide on how to
  use eth_feeHistory JSON-RPC in GetBlock Web3 documentation.
---

# eth\_feeHistory - Linea

This method returns historical base fees and priority-fee percentiles over a range of blocks. It is used to model fees for EIP-1559 transactions.

## Parameters

| Parameter         | Type   | Required | Description                                 |
| ----------------- | ------ | -------- | ------------------------------------------- |
| blockCount        | string | Yes      | Number of blocks in the range, hex-encoded  |
| newestBlock       | string | Yes      | Highest block of the range, hex or "latest" |
| rewardPercentiles | array  | No       | Percentiles to sample priority fees at      |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_feeHistory",
    "params": ["0x5", "latest", [25, 50, 75]],
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
    method: 'eth_feeHistory',
    params: ["0x5", "latest", [25, 50, 75]],
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
        'method': 'eth_feeHistory',
        'params': ["0x5", "latest", [25, 50, 75]],
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
            "method": "eth_feeHistory",
            "params": ["0x5", "latest", [25, 50, 75]],
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
        "oldestBlock": "0x1e847b",
        "baseFeePerGas": [
            "0x3b9aca00",
            "0x3b9aca00",
            "0x3b9aca00",
            "0x3b9aca00",
            "0x3b9aca00",
            "0x3b9aca00"
        ],
        "gasUsedRatio": [
            0.42,
            0.51,
            0.38,
            0.6,
            0.47
        ],
        "reward": [
            [
                "0x3b9aca00",
                "0x3b9aca00",
                "0x77359400"
            ]
        ]
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                       |
| --------- | ------ | ------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                 |
| id        | string | Request identifier matching the request           |
| result    | object | Fee history object over the requested block range |

### Result Object

| Field         | Type   | Description                                                 |
| ------------- | ------ | ----------------------------------------------------------- |
| oldestBlock   | string | Lowest block of the returned range, hex-encoded             |
| baseFeePerGas | array  | Base fee per gas for each block, including the next         |
| gasUsedRatio  | array  | Ratio of gas used to gas limit per block                    |
| reward        | array  | Priority-fee samples per block at the requested percentiles |

## Use Cases

* **Fee Modeling**: Model base-fee trends for EIP-1559 pricing
* **Tip Estimation**: Derive priority-fee tiers from historical percentiles
* **Congestion Analysis**: Read gas-used ratios to gauge network load
* **Wallet Fee UX**: Build fast, normal, and slow fee options

## Error Handling

| Error Code | Message        | Description                                            |
| ---------- | -------------- | ------------------------------------------------------ |
| -32602     | Invalid params | A parameter is missing or has the wrong type or format |
| -32603     | Internal error | The node failed to process the request                 |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const history = await provider.send('eth_feeHistory', ['0x5', 'latest', [25, 50, 75]]);
console.log(history);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { linea } from 'viem/chains';

const client = createPublicClient({ chain: linea, transport: http('https://go.getblock.io/<ACCESS-TOKEN>/') });

const history = await client.getFeeHistory({ blockCount: 5, rewardPercentiles: [25, 50, 75] });
console.log(history);
```
{% endcode %}
{% endtab %}
{% endtabs %}
