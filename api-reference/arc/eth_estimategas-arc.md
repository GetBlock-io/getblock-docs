---
description: >-
  Example code for the eth_estimateGas JSON-RPC method. Complete guide on how to
  use eth_estimateGas JSON-RPC in GetBlock Web3 documentation.
---

# eth\_estimateGas - ARC

This method estimates the gas required to execute a transaction without committing it. Use it to determine an appropriate `gas` value before broadcasting. Multiply by `eth_gasPrice` (also in USDC base units) to get the estimated total USDC cost.

## Parameters

| Parameter      | Type   | Required | Description                                                               |
| -------------- | ------ | -------- | ------------------------------------------------------------------------- |
| callObject     | object | Yes      | Transaction-shaped object with `to`, `data`, and optional `from`, `value` |
| blockParameter | string | No       | Optional block parameter (default: `"latest"`)                            |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_estimateGas",
    "params": [
        {
            "from": "0x4cef52a8f9d4b0c8b8d5f1a2b3c4d5e6f7a8b9c0",
            "to": "0xd85498dbeaeb1df24be52eed4f52eac2fbd56245",
            "value": "0xf4240"
        }
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
    "method": "eth_estimateGas",
    "params": [
        {
            "from": "0x4cef52a8f9d4b0c8b8d5f1a2b3c4d5e6f7a8b9c0",
            "to": "0xd85498dbeaeb1df24be52eed4f52eac2fbd56245",
            "value": "0xf4240"
        }
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
    "method": "eth_estimateGas",
    "params": [
        {
            "from": "0x4cef52a8f9d4b0c8b8d5f1a2b3c4d5e6f7a8b9c0",
            "to": "0xd85498dbeaeb1df24be52eed4f52eac2fbd56245",
            "value": "0xf4240"
        }
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
        "method": "eth_estimateGas",
        "params": [
                {
                        "from": "0x4cef52a8f9d4b0c8b8d5f1a2b3c4d5e6f7a8b9c0",
                        "to": "0xd85498dbeaeb1df24be52eed4f52eac2fbd56245",
                        "value": "0xf4240"
                }
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

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x5208"
}
```

## Response Parameters

| Field  | Type   | Description                        |
| ------ | ------ | ---------------------------------- |
| result | string | Estimated gas units required (hex) |

## Use Cases

* Setting an appropriate gas limit before signing a transaction
* Showing users an estimated USDC cost in wallet UIs
* Detecting transactions that would revert (estimate fails)
* Computing USDC fee budgets in server-side payment automation

## Error Handling

| Status Code | Error Message      | Cause                                                 |
| ----------- | ------------------ | ----------------------------------------------------- |
| 403         | Forbidden          | Missing or invalid `<ACCESS-TOKEN>`                   |
| -32602      | Invalid params     | Request parameters are missing or malformed           |
| -32601      | Method not found   | The method is not supported by this node              |
| 429         | Too Many Requests  | Rate limit exceeded for your plan                     |
| -32000      | Execution reverted | Transaction would revert; estimate cannot be returned |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// Most methods are exposed directly on the provider.
// For raw access, use provider.send(method, params).
const result = await provider.send('eth_estimateGas', [{"from": "0x4cef52a8f9d4b0c8b8d5f1a2b3c4d5e6f7a8b9c0", "to": "0xd85498dbeaeb1df24be52eed4f52eac2fbd56245", "value": "0xf4240"}]);
console.log(result);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code overflow="wrap" %}
```javascript
import { createPublicClient, http, defineChain } from 'viem';

const arcTestnet = defineChain({
    id: 5042002,
    name: 'Arc Testnet',
    network: 'arc-testnet',
    nativeCurrency: { name: 'USD Coin', symbol: 'USDC', decimals: 6 },
    rpcUrls: { default: { http: ['https://go.getblock.io/<ACCESS-TOKEN>/'] } },
    blockExplorers: { default: { name: 'arcscan', url: 'https://testnet.arcscan.app' } }
});

const client = createPublicClient({
    chain: arcTestnet,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const result = await client.request({ method: 'eth_estimateGas', params: [{"from": "0x4cef52a8f9d4b0c8b8d5f1a2b3c4d5e6f7a8b9c0", "to": "0xd85498dbeaeb1df24be52eed4f52eac2fbd56245", "value": "0xf4240"}] });
console.log(result);
```
{% endcode %}
{% endtab %}
{% endtabs %}
