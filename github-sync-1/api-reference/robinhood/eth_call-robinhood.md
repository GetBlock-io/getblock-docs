---
description: >-
  Example code for the eth_call JSON-RPC method. Complete guide to using the
  eth_call JSON-RPC method in GetBlock.io Web3 documentation.
---

# eth\_call - Robinhood

Executes a read-only contract call without creating a transaction. The canonical way to call view/pure functions on contracts — including reading Stock Token balances via `balanceOf(address)`. Does not consume gas (gas estimate is informational only) and does not modify state.

## Parameters

| Parameter  | Type     | Required | Description                                                                                               |
| ---------- | -------- | -------- | --------------------------------------------------------------------------------------------------------- |
| callObject | object   | Yes      | Call params: `from` (optional), `to`, `gas` (optional), `gasPrice` (optional), `value` (optional), `data` |
| block      | QUANTITY | TAG      | Yes                                                                                                       |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_call",
    "params": [
        {
            "to": "0x9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b",
            "data": "0x70a082310000000000000000000000007a25d0f0Ff6a25f36De0a19C3d5f6dC2C6EAeD6a"
        },
        "latest"
    ],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code overflow="wrap" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_call",
    "params": [
        {
            "to": "0x9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b",
            "data": "0x70a082310000000000000000000000007a25d0f0Ff6a25f36De0a19C3d5f6dC2C6EAeD6a"
        },
        "latest"
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
{% endcode %}
{% endtab %}

{% tab title="Python (Requests)" %}
{% code overflow="wrap" %}
```python
import requests
import json

url = "https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_call",
    "params": [
        {
            "to": "0x9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b",
            "data": "0x70a082310000000000000000000000007a25d0f0Ff6a25f36De0a19C3d5f6dC2C6EAeD6a"
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
{% endcode %}
{% endtab %}

{% tab title="Rust (Reqwest)" %}
{% code overflow="wrap" %}
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
                        "to": "0x9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b",
                        "data": "0x70a082310000000000000000000000007a25d0f0Ff6a25f36De0a19C3d5f6dC2C6EAeD6a"
                },
                "latest"
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
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x000000000000000000000000000000000000000000000000016345785d8a0000"
}
```
{% endcode %}

## Response Parameters

| Field  | Type | Description              |
| ------ | ---- | ------------------------ |
| result | DATA | ABI-encoded return value |

## Use Cases

* Reading Stock Token balances, total supply, name, symbol, decimals
* Calling view/pure functions on any contract
* Pre-flight simulation of state-changing calls (with caveats)

## Error Handling

<table data-search="false"><thead><tr><th>Status Code</th><th>Error Message</th><th>Cause</th></tr></thead><tbody><tr><td>403</td><td>Forbidden</td><td>Missing or invalid <code>&#x3C;ACCESS-TOKEN></code></td></tr><tr><td>-32602</td><td>Invalid params</td><td>Request parameters are missing or malformed</td></tr><tr><td>-32603</td><td>Internal error</td><td>Server-side error while processing the request</td></tr><tr><td>429</td><td>Too Many Requests</td><td>Rate limit exceeded for your plan</td></tr><tr><td>3</td><td>execution reverted</td><td>Contract execution reverted — the contract's logic rejected the call</td></tr><tr><td>-32602</td><td>Invalid call object</td><td>Malformed call parameters</td></tr></tbody></table>

## SDK Integration

{% tabs %}
{% tab title="ethers.js" %}
{% code overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/');

// Generic JSON-RPC call (works for any method):
const result = await provider.send('eth_call', [{"to": "0x9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b", "data": "0x70a082310000000000000000000000007a25d0f0Ff6a25f36De0a19C3d5f6dC2C6EAeD6a"}, "latest"]);
console.log(result);

// Many standard methods have typed wrappers on ethers Provider,
// e.g. provider.getBalance(addr), provider.getBlock(n), provider.getTransaction(hash).
```
{% endcode %}
{% endtab %}

{% tab title="viem" %}
{% code overflow="wrap" %}
```javascript
import { createPublicClient, http } from 'viem';

const client = createPublicClient({
    transport: http('https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/')
});

// Generic JSON-RPC request (works for any method):
const result = await client.request({
    method: 'eth_call',
    params: [{"to": "0x9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b", "data": "0x70a082310000000000000000000000007a25d0f0Ff6a25f36De0a19C3d5f6dC2C6EAeD6a"}, "latest"]
});
console.log(result);

// Many standard methods have typed wrappers on PublicClient,
// e.g. client.getBalance({ address }), client.getBlock({ blockNumber }).
```
{% endcode %}
{% endtab %}
{% endtabs %}
