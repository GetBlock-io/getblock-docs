---
description: >-
  Example code for the eth_call JSON-RPC method. Complete guide on how to use
  eth_call JSON-RPC in GetBlock Web3 documentation.
---

# eth\_call - Tempo

Executes a read-only call against contract state without committing a transaction. Use it to query TIP-20 balances, allowances, and DEX prices, or simulate state-changing calls without paying fees.

## Parameters

| Parameter      | Type   | Required | Description                                                                                  |
| -------------- | ------ | -------- | -------------------------------------------------------------------------------------------- |
| callObject     | object | Yes      | Transaction-shaped object with `to`, `data`, and optional `from`, `gas`, `gasPrice`, `value` |
| blockParameter | string | Yes      | Block number in hex, or block tag                                                            |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_call",
    "params": [
        {
            "to": "0x4242424242424242424242424242424242424242",
            "data": "0x70a08231000000000000000000000000d85498dbeaeb1df24be52eed4f52eac2fbd56245"
        },
        "latest"
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
    "method": "eth_call",
    "params": [
        {
            "to": "0x4242424242424242424242424242424242424242",
            "data": "0x70a08231000000000000000000000000d85498dbeaeb1df24be52eed4f52eac2fbd56245"
        },
        "latest"
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
    "method": "eth_call",
    "params": [
        {
            "to": "0x4242424242424242424242424242424242424242",
            "data": "0x70a08231000000000000000000000000d85498dbeaeb1df24be52eed4f52eac2fbd56245"
        },
        "latest"
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
        "method": "eth_call",
        "params": [
                {
                        "to": "0x4242424242424242424242424242424242424242",
                        "data": "0x70a08231000000000000000000000000d85498dbeaeb1df24be52eed4f52eac2fbd56245"
                },
                "latest"
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
    "result": "0x0000000000000000000000000000000000000000000000000000000005f5e100"
}
```
{% endcode %}

## Response Parameters

| Field  | Type   | Description                                           |
| ------ | ------ | ----------------------------------------------------- |
| result | string | ABI-encoded return value of the called function (hex) |

## Use Cases

* Reading TIP-20 stablecoin balances (the primary way to query Tempo balances)
* Querying Tempo stablecoin DEX prices and liquidity
* Simulating swaps and payment flows before broadcast
* Reading TIP-403 policy state for restricted assets

## Error Handling

| Status Code | Error Message      | Cause                                                                  |
| ----------- | ------------------ | ---------------------------------------------------------------------- |
| 403         | Forbidden          | Missing or invalid `<ACCESS-TOKEN>`                                    |
| -32602      | Invalid params     | Request parameters are missing or malformed                            |
| 429         | Too Many Requests  | Rate limit exceeded for your plan                                      |
| -32000      | Execution reverted | The contract call reverted; check the data field for the revert reason |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const result = await provider.send('eth_call', [{"to": "0x4242424242424242424242424242424242424242", "data": "0x70a08231000000000000000000000000d85498dbeaeb1df24be52eed4f52eac2fbd56245"}, "latest"]);
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

const result = await client.request({ method: 'eth_call', params: [{"to": "0x4242424242424242424242424242424242424242", "data": "0x70a08231000000000000000000000000d85498dbeaeb1df24be52eed4f52eac2fbd56245"}, "latest"] });
console.log(result);
```
{% endtab %}
{% endtabs %}
