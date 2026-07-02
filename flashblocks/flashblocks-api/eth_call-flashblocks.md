---
description: >-
  Example code for the eth_call - Flashblocks method. Complete guide on how to
  use eth_call - Flashblocks in GetBlock Web3 documentation.
---

# eth\_call - Flashblocks

This executes a read-only contract call without creating a transaction. When the block reference is `"pending"`, the call executes against state including every transaction preconfirmed into the latest Flashblock. This lets applications compute derived values (token balances, oracle prices, pool reserves) reflecting the freshest possible state.

## Parameters

| Parameter  | Type     | Required | Description                                                                                               |
| ---------- | -------- | -------- | --------------------------------------------------------------------------------------------------------- |
| callObject | object   | Yes      | Call params: `to`, `data`, `from` (optional), `gas` (optional), `gasPrice` (optional), `value` (optional) |
| block      | QUANTITY | TAG      | Yes                                                                                                       |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_call",
    "params": [
        {
            "to": "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913",
            "data": "0x70a082310000000000000000000000007a25d0f0ff6a25f36de0a19c3d5f6dc2c6eaed6a"
        },
        "pending"
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
    "method": "eth_call",
    "params": [
        {
            "to": "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913",
            "data": "0x70a082310000000000000000000000007a25d0f0ff6a25f36de0a19c3d5f6dc2c6eaed6a"
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
    "method": "eth_call",
    "params": [
        {
            "to": "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913",
            "data": "0x70a082310000000000000000000000007a25d0f0ff6a25f36de0a19c3d5f6dc2c6eaed6a"
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
        "method": "eth_call",
        "params": [
                {
                        "to": "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913",
                        "data": "0x70a082310000000000000000000000007a25d0f0ff6a25f36de0a19c3d5f6dc2c6eaed6a"
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

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x000000000000000000000000000000000000000000000000000000000098967f"
}
```

## Response Parameters

| Field  | Type | Description                                     |
| ------ | ---- | ----------------------------------------------- |
| result | DATA | ABI-encoded return value from the contract call |

## Use Cases

* Reading ERC-20 balances / allowances reflecting the latest preconfirmed transfers
* Fetching oracle prices with sub-block latency
* Pre-flight quotes for DEX swaps against the freshest reserves
* Any read that needs to reflect state 200ms ahead of the standard `latest` tag

## Error Handling

| Status Code | Error Message       | Cause                                                                |
| ----------- | ------------------- | -------------------------------------------------------------------- |
| 403         | Forbidden           | Missing or invalid `<ACCESS-TOKEN>`                                  |
| -32602      | Invalid params      | Request parameters are missing or malformed                          |
| -32603      | Internal error      | Server-side error while processing the request                       |
| 429         | Too Many Requests   | Rate limit exceeded for your plan                                    |
| 3           | execution reverted  | Contract execution reverted — the contract's logic rejected the call |
| -32602      | Invalid call object | Malformed call parameters                                            |

## SDK Integration

{% tabs %}
{% tab title="ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// Generic JSON-RPC call — 'pending' returns Flashblocks-preconfirmed state:
const result = await provider.send('eth_call', [{"to": "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913", "data": "0x70a082310000000000000000000000007a25d0f0ff6a25f36de0a19c3d5f6dc2c6eaed6a"}, "pending"]);
console.log(result);

// Many standard methods have typed wrappers on ethers Provider that accept 'pending':
// provider.getBalance(addr, 'pending'), provider.getTransactionCount(addr, 'pending'), etc.
```
{% endtab %}

{% tab title="viem" %}
```javascript
import { createPublicClient, http } from 'viem';

const client = createPublicClient({
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

// viem's read methods accept blockTag: 'pending' for Flashblocks-preconfirmed state:
const result = await client.request({
    method: 'eth_call',
    params: [{"to": "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913", "data": "0x70a082310000000000000000000000007a25d0f0ff6a25f36de0a19c3d5f6dc2c6eaed6a"}, "pending"]
});
console.log(result);

// Typed wrappers also accept blockTag: 'pending':
// client.getBalance({ address, blockTag: 'pending' })
// client.getBlock({ blockTag: 'pending' })
```
{% endtab %}
{% endtabs %}
