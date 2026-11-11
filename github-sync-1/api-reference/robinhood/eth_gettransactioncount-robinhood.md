---
description: >-
  Example code for the eth_getTransactionCount  JSON-RPC method. Complete guide
  to using the eth_getTransactionCount  JSON-RPC method in GetBlock.io Web3
  documentation.
---

# eth\_getTransactionCount - Robinhood

Returns the number of transactions sent from an address (the nonce). Required for constructing new transactions — each new transaction must use the next sequential nonce. Note: Robinhood Chain's 100ms block time means nonces can advance very quickly on active accounts.

## Parameters

| Parameter | Type            | Required | Description      |
| --------- | --------------- | -------- | ---------------- |
| address   | DATA (20 bytes) | Yes      | Address to check |
| block     | QUANTITY        | TAG      | Yes              |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getTransactionCount",
    "params": [
        "0x7a25d0f0Ff6a25f36De0a19C3d5f6dC2C6EAeD6a",
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
    "method": "eth_getTransactionCount",
    "params": [
        "0x7a25d0f0Ff6a25f36De0a19C3d5f6dC2C6EAeD6a",
        "pending"
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
    "method": "eth_getTransactionCount",
    "params": [
        "0x7a25d0f0Ff6a25f36De0a19C3d5f6dC2C6EAeD6a",
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
        "method": "eth_getTransactionCount",
        "params": [
                "0x7a25d0f0Ff6a25f36De0a19C3d5f6dC2C6EAeD6a",
                "pending"
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

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x42"
}
```

## Response Parameters

| Field  | Type     | Description                           |
| ------ | -------- | ------------------------------------- |
| result | QUANTITY | Transaction count / next nonce in hex |

## Use Cases

* Constructing transactions with the correct nonce
* Detecting account activity (counting transactions)
* Replacing pending transactions (use same nonce with higher gas price)

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
const result = await provider.send('eth_getTransactionCount', ["0x7a25d0f0Ff6a25f36De0a19C3d5f6dC2C6EAeD6a", "pending"]);
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
    method: 'eth_getTransactionCount',
    params: ["0x7a25d0f0Ff6a25f36De0a19C3d5f6dC2C6EAeD6a", "pending"]
});
console.log(result);

// Many standard methods have typed wrappers on PublicClient,
// e.g. client.getBalance({ address }), client.getBlock({ blockNumber }).
```
{% endtab %}
{% endtabs %}
