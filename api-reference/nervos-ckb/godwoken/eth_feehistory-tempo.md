# eth\_feehistory tempo

Returns historical base fees and priority fee percentiles over a window of recent blocks. Useful for building fee oracles, though Tempo's stable-fee design means the data is unusually flat.

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
        "oldestBlock": "0x9c3b",
        "baseFeePerGas": [
            "0x3b9aca00",
            "0x3b9aca00",
            "0x3b9aca00",
            "0x3b9aca00",
            "0x3b9aca00",
            "0x3b9aca00"
        ],
        "gasUsedRatio": [
            0.12,
            0.18,
            0.21,
            0.15,
            0.19
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

* Building EIP-1559 fee oracles for compatibility with Ethereum tooling
* Analytics on Tempo fee stability over time

## Error Handling

| Status Code | Error Message     | Cause                                                |
| ----------- | ----------------- | ---------------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                  |
| -32602      | Invalid params    | Request parameters are missing or malformed          |
| -32601      | Method not found  | Method does not exist or is not enabled on this node |
| 429         | Too Many Requests | Rate limit exceeded for your plan                    |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const result = await provider.send('eth_feeHistory', ["0x5", "latest", [25, 50, 75]]);
console.log(result);
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http, defineChain } from 'viem';

// Tempo is not yet in viem/chains as a built-in — define it inline.
const tempo = defineChain({
    id: 4217,
    name: 'Tempo',
    network: 'tempo',
    // Tempo has no native gas token. The `nativeCurrency` field is required by viem's
    // type system; values shown here are a no-op placeholder — actual fees are paid in TIP-20.
    nativeCurrency: { name: 'USD Placeholder', symbol: 'USD', decimals: 18 },
    rpcUrls: {
        default: { http: ['https://go.getblock.io/<ACCESS-TOKEN>/'] }
    },
    blockExplorers: { default: { name: 'Tempo Explorer', url: 'https://explore.tempo.xyz' } }
});

const client = createPublicClient({
    chain: tempo,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const result = await client.request({ method: 'eth_feeHistory', params: ["0x5", "latest", [25, 50, 75]] });
console.log(result);
```
{% endtab %}
{% endtabs %}
