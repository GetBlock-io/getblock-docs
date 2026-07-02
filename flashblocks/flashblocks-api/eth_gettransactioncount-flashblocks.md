---
description: >-
  Example code for the eth_getTransactionCount Flashblocks method. Complete
  guide on how to use eth_getTransactionCount Flashblocks in GetBlock Web3
  documentation.
---

# eth\_getTransactionCount - Flashblocks

This returns the transaction count (nonce) for an address. When called with `"pending"`, the returned nonce already accounts for transactions currently sitting in Flashblocks — preventing nonce collisions when an account submits many transactions in rapid succession.&#x20;

This is especially valuable for high-frequency trading bots and any application that fires more than one transaction per 2-second block.

## Parameters

| Parameter | Type            | Required | Description      |
| --------- | --------------- | -------- | ---------------- |
| address   | DATA (20 bytes) | Yes      | Address to check |
| block     | QUANTITY        | TAG      | Yes              |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
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
    "result": "0x42"
}
```
{% endcode %}

## Response Parameters

| Field  | Type     | Description                                                                  |
| ------ | -------- | ---------------------------------------------------------------------------- |
| result | QUANTITY | Transaction count / next nonce in hex, including transactions in Flashblocks |

## Use Cases

* Constructing the next transaction with the correct nonce, even during high-frequency submission
* Preventing 'nonce too low' errors when submitting multiple transactions per block
* Trading bot infrastructure — the primary use case for the pending nonce

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
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// Generic JSON-RPC call — 'pending' returns Flashblocks-preconfirmed state:
const result = await provider.send('eth_getTransactionCount', ["0x7a25d0f0Ff6a25f36De0a19C3d5f6dC2C6EAeD6a", "pending"]);
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
    method: 'eth_getTransactionCount',
    params: ["0x7a25d0f0Ff6a25f36De0a19C3d5f6dC2C6EAeD6a", "pending"]
});
console.log(result);

// Typed wrappers also accept blockTag: 'pending':
// client.getBalance({ address, blockTag: 'pending' })
// client.getBlock({ blockTag: 'pending' })
```
{% endtab %}
{% endtabs %}
