---
description: >-
  Example code for the eth_feeHistory JSON-RPC method. Complete guide to using
  the eth_feeHistory JSON-RPC method in GetBlock.io Web3 documentation.
---

# eth\_feeHistory - Robinhood

Returns historical gas fee data for analyzing fee market trends — base fees, reward percentiles, and gas usage ratios across a range of blocks. Given Robinhood Chain's 100ms blocks, a small block count covers a very short time window.

## Parameters

| Parameter         | Type            | Required | Description                                                  |
| ----------------- | --------------- | -------- | ------------------------------------------------------------ |
| blockCount        | QUANTITY        | Yes      | Number of blocks in the requested range (max 1024)           |
| newestBlock       | QUANTITY        | TAG      | Yes                                                          |
| rewardPercentiles | ARRAY of NUMBER | No       | Percentiles of priority fees to sample (e.g. `[25, 50, 75]`) |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_feeHistory",
    "params": [
        "0xa",
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

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_feeHistory",
    "params": [
        "0xa",
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
    url: 'https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/',
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

url = "https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_feeHistory",
    "params": [
        "0xa",
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
                "0xa",
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
        .post("https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/")
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
        "oldestBlock": "0x124f76",
        "baseFeePerGas": [
            "0x5f5e100",
            "0x5f5e100",
            "0x5f5e100",
            "0x5f5e100",
            "0x5f5e100",
            "0x5f5e100",
            "0x5f5e100",
            "0x5f5e100",
            "0x5f5e100",
            "0x5f5e100",
            "0x5f5e100"
        ],
        "gasUsedRatio": [
            0.15,
            0.18,
            0.22,
            0.19,
            0.24,
            0.21,
            0.17,
            0.2,
            0.23,
            0.16
        ],
        "reward": [
            [
                "0x0"
            ],
            [
                "0x0"
            ],
            [
                "0x0"
            ],
            [
                "0x0"
            ],
            [
                "0x0"
            ],
            [
                "0x0"
            ],
            [
                "0x0"
            ],
            [
                "0x0"
            ],
            [
                "0x0"
            ],
            [
                "0x0"
            ]
        ]
    }
}
```
{% endcode %}

## Response Parameters

| Field                | Type                 | Description                                            |
| -------------------- | -------------------- | ------------------------------------------------------ |
| result.oldestBlock   | QUANTITY             | Lowest block number in the returned range              |
| result.baseFeePerGas | array of QUANTITY    | Base fee for each block + 1 element for the next block |
| result.gasUsedRatio  | array of number      | Fraction of gas used vs gas limit per block            |
| result.reward        | 2D array of QUANTITY | Priority fees at each requested percentile per block   |

## Use Cases

* Optimal EIP-1559 fee selection
* Building fee oracle services
* Historical gas market analysis

## Error Handling

| Status Code | Error Message     | Cause                                          |
| ----------- | ----------------- | ---------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`            |
| -32602      | Invalid params    | Request parameters are missing or malformed    |
| -32603      | Internal error    | Server-side error while processing the request |
| 429         | Too Many Requests | Rate limit exceeded for your plan              |

## SDK Integration

{% tabs %}
{% tab title="ethers.js" %}
{% code overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/');

// Generic JSON-RPC call (works for any method):
const result = await provider.send('eth_feeHistory', ["0xa", "latest", [25, 50, 75]]);
console.log(result);

// Many standard methods have typed wrappers on ethers Provider,
// e.g. provider.getBalance(addr), provider.getBlock(n), provider.getTransaction(hash).
```
{% endcode %}
{% endtab %}

{% tab title="viem" %}
```javascript
import { createPublicClient, http } from 'viem';

const client = createPublicClient({
    transport: http('https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/')
});

// Generic JSON-RPC request (works for any method):
const result = await client.request({
    method: 'eth_feeHistory',
    params: ["0xa", "latest", [25, 50, 75]]
});
console.log(result);

// Many standard methods have typed wrappers on PublicClient,
// e.g. client.getBalance({ address }), client.getBlock({ blockNumber }).
```
{% endtab %}
{% endtabs %}
