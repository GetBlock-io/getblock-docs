---
description: >-
  Example code for the eth_feeHistory JSON-RPC method. Сomplete guide on how to
  use eth_feeHistory JSON-RPC in GetBlock.io Web3 documentation.
---

# eth\_feeHistory - zkSync

Returns historical base fees and priority fee percentiles over a window of recent blocks. Use it to build a fee oracle that adapts to recent L2 network conditions.

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
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
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
    "result": {
        "oldestBlock": "0x2249b3",
        "baseFeePerGas": [
            "0xee6b280",
            "0xee6b280",
            "0xee6b280",
            "0xee6b280",
            "0xee6b280",
            "0xee6b280"
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
                "0x0",
                "0x0",
                "0x0"
            ],
            [
                "0x0",
                "0x0",
                "0x0"
            ],
            [
                "0x0",
                "0x0",
                "0x0"
            ],
            [
                "0x0",
                "0x0",
                "0x0"
            ],
            [
                "0x0",
                "0x0",
                "0x0"
            ]
        ]
    }
}
```
{% endcode %}

## Response Parameters

| Field                | Type                     | Description                                                  |
| -------------------- | ------------------------ | ------------------------------------------------------------ |
| result.oldestBlock   | string                   | Oldest block in the returned window (hex)                    |
| result.baseFeePerGas | array of string          | Base fee per gas for each block in the window plus one extra |
| result.gasUsedRatio  | array of number          | Ratio of gas used to gas limit for each block                |
| result.reward        | array of array of string | Priority fee percentiles for each block                      |

## Use Cases

* Building EIP-1559 fee oracles
* Adaptive gas pricing in production wallets
* Analytics on fee market trends

## Error Handling

| Status Code | Error Message     | Cause                                       |
| ----------- | ----------------- | ------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`         |
| -32602      | Invalid params    | Request parameters are missing or malformed |
| 429         | Too Many Requests | Rate limit exceeded for your plan           |

## SDK Integration

{% tabs %}
{% tab title="zksync-ethers (JavaScript)" %}
```javascript
import { Provider } from 'zksync-ethers';

const provider = new Provider('https://go.getblock.io/<ACCESS-TOKEN>/');

// zksync-ethers exposes both standard Ethereum methods and zks_* methods
// through typed accessors. For raw access:
const result = await provider.send('eth_feeHistory', ["0x5", "latest", [25, 50, 75]]);
console.log(result);
```
{% endtab %}

{% tab title="zksync2-python (Python)" %}
```python
from zksync2.module.module_builder import ZkSyncBuilder

zk_web3 = ZkSyncBuilder.build('https://go.getblock.io/<ACCESS-TOKEN>/')

# zksync2-python exposes the JSON-RPC layer directly.
result = zk_web3.zksync._zks_endpoints if 'eth_feeHistory'.startswith('zks_') else None
# Or call any method generically:
result = zk_web3.provider.make_request('eth_feeHistory', ["0x5", "latest", [25, 50, 75]])
print(result)
```
{% endtab %}
{% endtabs %}
