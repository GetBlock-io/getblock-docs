---
description: >-
  Example code for the eth_simulateV1 JSON RPC method. Сomplete guide on how to
  use eth_simulateV1 JSON RPC in GetBlock Web3 documentation.
---

# eth\_simulateV1 - Ethereum

This method simulates a bundle of transactions against a specific block state and returns per-transaction execution results including gas used, return data, and event logs. Unlike `eth_call` (single message call) or `eth_estimateGas` (single-transaction gas), it simulates chained transactions in a single call. Used heavily by wallets, MEV searchers, and bundlers to preview complex multi-step operations before submission.

## Parameters

| Parameter        | Type   | Required | Description                                                                          |
| ---------------- | ------ | -------- | ------------------------------------------------------------------------------------ |
| `payload`        | object | Yes      | Simulation payload (see below)                                                       |
| `blockParameter` | string | Yes      | Block state to simulate against — hex block number, or `latest`, `finalized`, `safe` |

### Payload Object

| Field                    | Type            | Required | Description                                                                |
| ------------------------ | --------------- | -------- | -------------------------------------------------------------------------- |
| `blockStateCalls`        | array of object | Yes      | Ordered list of block simulations, each containing transactions to execute |
| `traceTransfers`         | boolean         | No       | If `true`, emit synthetic Transfer events for ETH movements                |
| `validation`             | boolean         | No       | If `true`, apply signature and nonce validation (defaults to `false`)      |
| `returnFullTransactions` | boolean         | No       | If `true`, return full transaction objects in the response                 |

### Block State Call Object

| Field            | Type            | Required | Description                                                                                   |
| ---------------- | --------------- | -------- | --------------------------------------------------------------------------------------------- |
| `blockOverrides` | object          | No       | Overrides for block-level fields (`number`, `time`, `baseFeePerGas`, etc.)                    |
| `stateOverrides` | object          | No       | Overrides for account state (`balance`, `nonce`, `code`, `storage`)                           |
| `calls`          | array of object | Yes      | Transactions to execute in this simulated block (same shape as `eth_call` transaction object) |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_simulateV1",
    "params": [
        {
            "blockStateCalls": [
                {
                    "calls": [
                        {
                            "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
                            "to": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
                            "data": "0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045"
                        }
                    ]
                }
            ],
            "validation": false,
            "traceTransfers": true
        },
        "latest"
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
    method: 'eth_simulateV1',
    params: [
        {
            "blockStateCalls": [
                {
                    "calls": [
                        {
                            "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
                            "to": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
                            "data": "0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045"
                        }
                    ]
                }
            ],
            "validation": false,
            "traceTransfers": true
        },
        "latest"
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
        'method': 'eth_simulateV1',
        'params': [
        {
            "blockStateCalls": [
                {
                    "calls": [
                        {
                            "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
                            "to": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
                            "data": "0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045"
                        }
                    ]
                }
            ],
            "validation": false,
            "traceTransfers": true
        },
        "latest"
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
            "method": "eth_simulateV1",
            "params": [
        {
            "blockStateCalls": [
                {
                    "calls": [
                        {
                            "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
                            "to": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
                            "data": "0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045"
                        }
                    ]
                }
            ],
            "validation": false,
            "traceTransfers": true
        },
        "latest"
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
    "result": [
        {
            "number": "0x1720340",
            "hash": "0x9ec8e2f6b78d8f5f2ac3e5d61b0d38d4d5a8f0e7c8b5a2f9d3e6c1b7a4d0f2e5c",
            "timestamp": "0x67abcdef",
            "gasLimit": "0x39dc7fb",
            "gasUsed": "0x8ba0",
            "baseFeePerGas": "0x5f5e100",
            "calls": [
                {
                    "status": "0x1",
                    "returnData": "0x0000000000000000000000000000000000000000000000000de0b6b3a7640000",
                    "gasUsed": "0x8ba0",
                    "logs": []
                }
            ]
        }
    ]
}
```

## Response Parameters

| Parameter                     | Type            | Description                                                   |
| ----------------------------- | --------------- | ------------------------------------------------------------- |
| `jsonrpc`                     | string          | JSON-RPC protocol version ("2.0")                             |
| `id`                          | string          | Request identifier matching the request                       |
| `result`                      | array of object | Simulated blocks, one per `blockStateCall` entry              |
| `result[].number`             | string          | Simulated block number (hex)                                  |
| `result[].hash`               | string          | Simulated block hash                                          |
| `result[].timestamp`          | string          | Simulated block timestamp                                     |
| `result[].gasUsed`            | string          | Total gas consumed by all calls in this simulated block       |
| `result[].baseFeePerGas`      | string          | Base fee per gas for this simulated block                     |
| `result[].calls`              | array of object | Per-call execution results                                    |
| `result[].calls[].status`     | string          | `0x1` = success, `0x0` = reverted                             |
| `result[].calls[].returnData` | string          | Hex-encoded return data (or revert reason for reverted calls) |
| `result[].calls[].gasUsed`    | string          | Gas consumed by this call                                     |
| `result[].calls[].logs`       | array of object | Event logs emitted during this call                           |

## Use Cases

* **Multi-Step Transaction Preview**: Preview the outcome of a swap, approve+swap, or multi-hop DeFi operation before broadcasting
* **MEV Bundle Simulation**: Simulate an entire MEV bundle to verify profitability and behavior before submission to a builder
* **Wallet UX Preview**: Show users a rich 'this is what will happen' preview covering multiple contract interactions
* **Account Abstraction Preflight**: ERC-4337 bundlers and paymasters simulate UserOperations before submission
* **What-If Analysis**: State-overrides enable simulating actions with hypothetical account balances or contract code

## Error Handling

| Error Code | Message            | Description                                                                                                |
| ---------- | ------------------ | ---------------------------------------------------------------------------------------------------------- |
| -32602     | Invalid params     | Payload structure is malformed or a call object is invalid                                                 |
| 3          | Execution reverted | One or more simulated calls reverted (individual `calls[].status = "0x0"` also indicates per-call reverts) |
| -32000     | Execution error    | Node failed to simulate the payload                                                                        |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const result = await provider.send('eth_simulateV1', [
    {
        blockStateCalls: [{
            calls: [{
                from: '0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045',
                to: '0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2',
                data: '0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045',
            }],
        }],
        traceTransfers: true,
    },
    'latest',
]);
const simCall = result[0].calls[0];
console.log('Status:', simCall.status === '0x1' ? 'success' : 'reverted');
console.log('Return:', simCall.returnData);
console.log('Gas used:', simCall.gasUsed);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { mainnet } from 'viem/chains';

const client = createPublicClient({
    chain: mainnet,
    transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/'),
});

const result = await client.request({
    method: 'eth_simulateV1',
    params: [
        {
            blockStateCalls: [{
                calls: [{
                    from: '0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045',
                    to: '0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2',
                    data: '0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045',
                }],
            }],
            traceTransfers: true,
        },
        'latest',
    ],
});
console.log(result[0].calls[0]);
```
{% endcode %}
{% endtab %}
{% endtabs %}
