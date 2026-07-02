---
description: >-
  Example code for the eth_simulateV1 - Flashblocks method. Complete guide on
  how to use eth_simulateV1 - Flashblocks in GetBlock Web3 documentation.
---

# eth\_simulateV1 - Flashblocks

This simulates a bundle of transactions against the latest Flashblock — including state overrides, transfer tracing, and full validation. This is critical for MEV searchers, liquidation bots, and any application that needs to know "what would happen if I submitted this bundle right now, given the preconfirmed state?" The simulation reflects every transaction already committed to the in-progress block.

## Parameters

| Parameter        | Type     | Required | Description                                                                                                |
| ---------------- | -------- | -------- | ---------------------------------------------------------------------------------------------------------- |
| simulationParams | object   | Yes      | Simulation object: `blockStateCalls` (array of call bundles), `traceTransfers` (bool), `validation` (bool) |
| block            | QUANTITY | TAG      | Yes                                                                                                        |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
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
                            "to": "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913",
                            "data": "0xa9059cbb0000000000000000000000007a25d0f0ff6a25f36de0a19c3d5f6dc2c6eaed6a00000000000000000000000000000000000000000000000000000000000186a0",
                            "from": "0x7a25d0f0Ff6a25f36De0a19C3d5f6dC2C6EAeD6a"
                        }
                    ],
                    "stateOverrides": {}
                }
            ],
            "traceTransfers": true,
            "validation": true
        },
        "pending"
    ],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_simulateV1",
    "params": [
        {
            "blockStateCalls": [
                {
                    "calls": [
                        {
                            "to": "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913",
                            "data": "0xa9059cbb0000000000000000000000007a25d0f0ff6a25f36de0a19c3d5f6dc2c6eaed6a00000000000000000000000000000000000000000000000000000000000186a0",
                            "from": "0x7a25d0f0Ff6a25f36De0a19C3d5f6dC2C6EAeD6a"
                        }
                    ],
                    "stateOverrides": {}
                }
            ],
            "traceTransfers": true,
            "validation": true
        },
        "pending"
    ],
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers: {
        'Content-Type': 'application/json'
    },
    data: data
};

axios(config)
    .then(response => console.log(JSON.stringify(response.data, null, 2)))
    .catch(error => console.log(error));
```
{% endtab %}

{% tab title="Python (Requests)" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_simulateV1",
    "params": [
        {
            "blockStateCalls": [
                {
                    "calls": [
                        {
                            "to": "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913",
                            "data": "0xa9059cbb0000000000000000000000007a25d0f0ff6a25f36de0a19c3d5f6dc2c6eaed6a00000000000000000000000000000000000000000000000000000000000186a0",
                            "from": "0x7a25d0f0Ff6a25f36De0a19C3d5f6dC2C6EAeD6a"
                        }
                    ],
                    "stateOverrides": {}
                }
            ],
            "traceTransfers": true,
            "validation": true
        },
        "pending"
    ],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endtab %}

{% tab title="Rust (Reqwest)" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let payload = json!({
        "jsonrpc": "2.0",
        "method": "eth_simulateV1",
        "params": [
                {
                        "blockStateCalls": [
                                {
                                        "calls": [
                                                {
                                                        "to": "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913",
                                                        "data": "0xa9059cbb0000000000000000000000007a25d0f0ff6a25f36de0a19c3d5f6dc2c6eaed6a00000000000000000000000000000000000000000000000000000000000186a0",
                                                        "from": "0x7a25d0f0Ff6a25f36De0a19C3d5f6dC2C6EAeD6a"
                                                }
                                        ],
                                        "stateOverrides": {}
                                }
                        ],
                        "traceTransfers": true,
                        "validation": true
                },
                "pending"
        ],
        "id": "getblock.io"
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

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": [
        {
            "number": "0x16e3600",
            "hash": "0x0000000000000000000000000000000000000000000000000000000000000000",
            "timestamp": "0x68f3b85f",
            "gasLimit": "0x3938700",
            "gasUsed": "0x5208",
            "calls": [
                {
                    "status": "0x1",
                    "gasUsed": "0x5208",
                    "returnData": "0x0000000000000000000000000000000000000000000000000000000000000001",
                    "logs": [
                        {
                            "address": "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913",
                            "topics": [
                                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"
                            ],
                            "data": "0x00000000000000000000000000000000000000000000000000000000000186a0"
                        }
                    ]
                }
            ]
        }
    ]
}
```
{% endcode %}

## Response Parameters

| Field                         | Type     | Description                                                |
| ----------------------------- | -------- | ---------------------------------------------------------- |
| result                        | array    | Simulated block(s) — one entry per block state call bundle |
| result\[].calls               | array    | Per-call simulation result                                 |
| result\[].calls\[].status     | QUANTITY | `0x1` = success, `0x0` = reverted                          |
| result\[].calls\[].gasUsed    | QUANTITY | Gas consumed by the call                                   |
| result\[].calls\[].returnData | DATA     | ABI-encoded return value                                   |
| result\[].calls\[].logs       | array    | Event logs the call would emit                             |

## Use Cases

* MEV searchers testing bundle profitability against preconfirmed state
* Liquidation bots validating a liquidation call would succeed at the current preconfirmed price
* DEX aggregators simulating multi-hop swap routes against the freshest reserves
* Wallet UX preflight — showing users the projected outcome of their transaction before signing

## Error Handling

| Status Code | Error Message                 | Cause                                          |
| ----------- | ----------------------------- | ---------------------------------------------- |
| 403         | Forbidden                     | Missing or invalid `<ACCESS-TOKEN>`            |
| -32602      | Invalid params                | Request parameters are missing or malformed    |
| -32603      | Internal error                | Server-side error while processing the request |
| 429         | Too Many Requests             | Rate limit exceeded for your plan              |
| 3           | execution reverted            | One or more calls in the simulation reverted   |
| -32602      | Invalid simulation parameters | Malformed simulation object                    |

## SDK Integration

{% tabs %}
{% tab title="ethers.js" %}
{% code overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// Flashblocks-specific methods use the raw send interface:
const result = await provider.send('eth_simulateV1', [{"blockStateCalls": [{"calls": [{"to": "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913", "data": "0xa9059cbb0000000000000000000000007a25d0f0ff6a25f36de0a19c3d5f6dc2c6eaed6a00000000000000000000000000000000000000000000000000000000000186a0", "from": "0x7a25d0f0Ff6a25f36De0a19C3d5f6dC2C6EAeD6a"}], "stateOverrides": {}}], "traceTransfers": true, "validation": true}, "pending"]);
console.log(result);
```
{% endcode %}
{% endtab %}

{% tab title="viem" %}
{% code overflow="wrap" %}
```javascript
import { createPublicClient, http } from 'viem';

const client = createPublicClient({
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

// Flashblocks-specific methods need the raw request transport:
const result = await client.request({
    method: 'eth_simulateV1',
    params: [{"blockStateCalls": [{"calls": [{"to": "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913", "data": "0xa9059cbb0000000000000000000000007a25d0f0ff6a25f36de0a19c3d5f6dc2c6eaed6a00000000000000000000000000000000000000000000000000000000000186a0", "from": "0x7a25d0f0Ff6a25f36De0a19C3d5f6dC2C6EAeD6a"}], "stateOverrides": {}}], "traceTransfers": true, "validation": true}, "pending"]
});
console.log(result);
```
{% endcode %}
{% endtab %}
{% endtabs %}
