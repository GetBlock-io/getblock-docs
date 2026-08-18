---
description: >-
  Example code for the eth_feeHistory JSON RPC method. Сomplete guide on how to
  use eth_feeHistory JSON RPC in GetBlock Web3 documentation.
---

# eth\_feeHistory - Ethereum

This method returns historical gas fee data over a range of recent blocks. Given a block count, the last block to include, and reward percentiles, it returns the base fee for each block plus priority-fee reward percentiles from included transactions. This is the primary input for well-calibrated EIP-1559 fee estimation strategies used by modern wallets.

## Parameters

| Parameter           | Type             | Required | Description                                                                                            |
| ------------------- | ---------------- | -------- | ------------------------------------------------------------------------------------------------------ |
| `blockCount`        | string           | Yes      | Hex-encoded number of blocks to look back (max 1024)                                                   |
| `newestBlock`       | string           | Yes      | Newest block to include — block number in hex, or `latest`, `earliest`, `pending`, `finalized`, `safe` |
| `rewardPercentiles` | array of numbers | No       | Priority-fee percentiles (0-100) to return per block (e.g. `[25, 50, 75]`)                             |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_feeHistory",
    "params": [
        "0x5",
        "latest",
        [
            25,
            50,
            75
        ]
    ],
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
    method: 'eth_feeHistory',
    params: [
        "0x5",
        "latest",
        [
            25,
            50,
            75
        ]
    ],
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
        'method': 'eth_feeHistory',
        'params': [
            "0x5",
            "latest",
            [
                25,
                50,
                75
            ]
        ],
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
            "method": "eth_feeHistory",
            "params": [
                "0x5",
                "latest",
                [
                    25,
                    50,
                    75
                ]
            ],
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
    "oldestBlock": "0x172033c",
    "baseFeePerGas": [
      "0x5f5e100",
      "0x60a3f80",
      "0x5d21dba",
      "0x5b8d7c1",
      "0x5cc4e56",
      "0x5e0c1a3"
    ],
    "gasUsedRatio": [
      0.512,
      0.482,
      0.501,
      0.633,
      0.517,
      0.499
    ],
    "baseFeePerBlobGas": [
      "0x1",
      "0x1",
      "0x1",
      "0x1",
      "0x1",
      "0x1"
    ],
    "blobGasUsedRatio": [
      0.428,
      0.571,
      0.5,
      0.428,
      0.357,
      0.5
    ],
    "reward": [
      [
        "0x3b9aca00",
        "0x77359400",
        "0xba43b7400"
      ],
      [
        "0x3b9aca00",
        "0x77359400",
        "0xba43b7400"
      ],
      [
        "0x3b9aca00",
        "0x77359400",
        "0xba43b7400"
      ],
      [
        "0x3b9aca00",
        "0x77359400",
        "0xba43b7400"
      ],
      [
        "0x3b9aca00",
        "0x77359400",
        "0xba43b7400"
      ]
    ]
  }
}
```

## Response Parameters

| Parameter                  | Type                     | Description                                                                 |
| -------------------------- | ------------------------ | --------------------------------------------------------------------------- |
| `jsonrpc`                  | string                   | JSON-RPC protocol version ("2.0")                                           |
| `id`                       | string                   | Request identifier matching the request                                     |
| `result.oldestBlock`       | string                   | Hex-encoded block number of the oldest block in the returned window         |
| `result.baseFeePerGas`     | array of string          | Base fee (wei, hex) for each block in the window plus the next block        |
| `result.gasUsedRatio`      | array of number          | Ratio of gas used to gas limit per block (0.0 - 1.0)                        |
| `result.baseFeePerBlobGas` | array of string          | Post-Cancun blob base fee (wei, hex) per block                              |
| `result.blobGasUsedRatio`  | array of number          | Ratio of blob gas used to blob gas target per block (0.0 - 1.0)             |
| `result.reward`            | array of array of string | Priority-fee percentiles (wei, hex) per block for the requested percentiles |

## Use Cases

* **Well-Calibrated Fee Estimation**: Wallets compute wallet-recommended fees from historical rewards distribution rather than a single-value estimate
* **Congestion Analysis**: Detect network congestion from `gasUsedRatio` and `baseFeePerGas` trajectory
* **Blob Fee Trend Analysis**: Track blob base-fee trajectory and blob-gas-used ratios for L2 cost modeling
* **Historical Fee Charts**: Wallet UIs displaying fee-history charts to help users choose an appropriate fee level

## Error Handling

| Error Code | Message        | Description                                                                                   |
| ---------- | -------------- | --------------------------------------------------------------------------------------------- |
| -32602     | Invalid params | `blockCount` exceeds 1024, or `rewardPercentiles` contains non-numeric or out-of-range values |
| -32603     | Internal error | Node failed to compile the fee history window                                                 |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const feeHistory = await provider.send('eth_feeHistory', [
    '0x5',       // last 5 blocks
    'latest',    // ending at latest
    [25, 50, 75] // reward percentiles
]);
console.log('Oldest block:', feeHistory.oldestBlock);
console.log('Base fees:', feeHistory.baseFeePerGas);
console.log('Reward percentiles:', feeHistory.reward);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" overflow="wrap" %}
```javascript
import { createPublicClient, http } from 'viem';
import { mainnet } from 'viem/chains';

const client = createPublicClient({
    chain: mainnet,
    transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/'),
});

const feeHistory = await client.getFeeHistory({
    blockCount: 5,
    rewardPercentiles: [25, 50, 75],
});
console.log('Base fees:', feeHistory.baseFeePerGas);
console.log('Reward percentiles:', feeHistory.reward);
```
{% endcode %}
{% endtab %}
{% endtabs %}
