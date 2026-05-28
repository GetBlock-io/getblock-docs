---
description: >-
  Example code for the eth_feeHistory JSON-RPC method. Complete guide on how to
  use eth_feeHistory JSON-RPC in GetBlock Web3 documentation.
---

# eth\_feeHistory - AVAX

Returns historical base fees and priority fee percentiles over a window of recent C-Chain blocks. Use it to build a fee oracle that adapts to recent network conditions.

## Parameters

| Parameter         | Type            | Required | Description                                                     |
| ----------------- | --------------- | -------- | --------------------------------------------------------------- |
| blockCount        | string          | Yes      | Number of blocks in the window (hex, max 1024)                  |
| newestBlock       | string          | Yes      | The newest block in the window — hex number or tag              |
| rewardPercentiles | array of number | Yes      | Percentiles to compute over priority fees (e.g. `[25, 50, 75]`) |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc' \
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
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
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
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc',
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

url = "https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc"

payload = json.dumps({
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
});

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc")
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
    "result": {
        "oldestBlock": "0x44b8a37",
        "baseFeePerGas": [
            "0x5d21dba00",
            "0x5d21dba00",
            "0x5d21dba00",
            "0x5d21dba00",
            "0x5d21dba00",
            "0x5d21dba00"
        ],
        "gasUsedRatio": [
            0.21,
            0.18,
            0.34,
            0.27,
            0.15
        ],
        "reward": [
            [
                "0x5d21dba00",
                "0x5d21dba00",
                "0x5d21dba00"
            ],
            [
                "0x5d21dba00",
                "0x5d21dba00",
                "0x5d21dba00"
            ],
            [
                "0x5d21dba00",
                "0x5d21dba00",
                "0x5d21dba00"
            ],
            [
                "0x5d21dba00",
                "0x5d21dba00",
                "0x5d21dba00"
            ],
            [
                "0x5d21dba00",
                "0x5d21dba00",
                "0x5d21dba00"
            ]
        ]
    }
}
```
{% endcode %}

## Response Parameters

| Field                | Type                     | Description                                   |
| -------------------- | ------------------------ | --------------------------------------------- |
| result.oldestBlock   | string                   | Oldest block in the returned window (hex)     |
| result.baseFeePerGas | array of string          | Base fee per gas for each block (length n+1)  |
| result.gasUsedRatio  | array of number          | Ratio of gas used to gas limit for each block |
| result.reward        | array of array of string | Priority fee percentiles for each block       |

## Use Cases

* Building EIP-1559 fee oracles
* Adaptive gas pricing in production wallets
* Analytics on Avalanche fee market trends

## Error Handling

| Status Code | Error Message     | Cause                                       |
| ----------- | ----------------- | ------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`         |
| -32602      | Invalid params    | Request parameters are missing or malformed |
| 429         | Too Many Requests | Rate limit exceeded for your plan           |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc');

// Most methods are exposed directly on the provider.
// For raw access, use provider.send(method, params).
const result = await provider.send('eth_feeHistory', ["0x5", "latest", [25, 50, 75]]);
console.log(result);
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { avalanche } from 'viem/chains';

const client = createPublicClient({
    chain: avalanche,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc')
});

const result = await client.request({ method: 'eth_feeHistory', params: ["0x5", "latest", [25, 50, 75]] });
console.log(result);
```
{% endtab %}
{% endtabs %}
