---
description: >-
  Example code for the eth_suggestPriceOptions JSON-RPC method. Complete guide
  on how to use eth_suggestPriceOptions JSON-RPC in GetBlock Web3 documentation.
---

# eth\_suggestPriceOptions - AVAX

Returns suggested gas price options for slow, normal, and fast confirmation tiers. Avalanche-specific extension that gives wallets a one-call replacement for building their own multi-tier fee oracle.

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_suggestPriceOptions",
    "params": [],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_suggestPriceOptions",
    "params": [],
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
    "method": "eth_suggestPriceOptions",
    "params": [],
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
        "method": "eth_suggestPriceOptions",
        "params": [],
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
        "slow": {
            "maxFeePerGas": "0x5d21dba00",
            "maxPriorityFeePerGas": "0x3b9aca00"
        },
        "normal": {
            "maxFeePerGas": "0x77359400",
            "maxPriorityFeePerGas": "0x59682f00"
        },
        "fast": {
            "maxFeePerGas": "0x9502f9000",
            "maxPriorityFeePerGas": "0x77359400"
        }
    }
}
```
{% endcode %}

## Response Parameters

| Field                        | Type   | Description                                        |
| ---------------------------- | ------ | -------------------------------------------------- |
| result.slow                  | object | Slow tier fee suggestion (\~30s+ confirmation)     |
| result.normal                | object | Normal tier fee suggestion (\~2s confirmation)     |
| result.fast                  | object | Fast tier fee suggestion (next-block confirmation) |
| result..maxFeePerGas         | string | Suggested max fee per gas (wei, hex)               |
| result..maxPriorityFeePerGas | string | Suggested max priority fee per gas (wei, hex)      |

## Use Cases

* Wallet UX showing slow/normal/fast fee options
* Single-call multi-tier fee oracle
* Simplifying EIP-1559 fee construction in client libraries

## Error Handling

| Status Code | Error Message     | Cause                                       |
| ----------- | ----------------- | ------------------------------------------- |
| 404         | Not Found         | Missing or invalid `<ACCESS-TOKEN>`         |
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
const result = await provider.send('eth_suggestPriceOptions', []);
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

const result = await client.request({ method: 'eth_suggestPriceOptions', params: [] });
console.log(result);
```
{% endtab %}
{% endtabs %}
