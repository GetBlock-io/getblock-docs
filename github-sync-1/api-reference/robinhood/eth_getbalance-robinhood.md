---
description: >-
  Example code for the eth_getBalance JSON-RPC method. Complete guide to using
  the eth_getBalance JSON-RPC method in GetBlock.io Web3 documentation.
---

# eth\_getBalance - Robinhood

Returns the ETH balance of an address in wei (18 decimals). Pass `latest`, `safe`, `finalized`, `pending`, `earliest`, or a specific block number (hex) for the block reference. On Arbitrum Orbit, `finalized` means the block has been posted to L1 and finalized there — typically several minutes behind `latest`.

## Parameters

| Parameter | Type            | Required | Description                                                                  |
| --------- | --------------- | -------- | ---------------------------------------------------------------------------- |
| address   | DATA (20 bytes) | Yes      | Address to check                                                             |
| block     | QUANTITY \| TAG | Yes      | `latest`, `safe`, `finalized`, `pending`, `earliest`, or block number in hex |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBalance",
    "params": [
        "0x7a25d0f0Ff6a25f36De0a19C3d5f6dC2C6EAeD6a",
        "latest"
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
    "method": "eth_getBalance",
    "params": [
        "0x7a25d0f0Ff6a25f36De0a19C3d5f6dC2C6EAeD6a",
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
{% endtab %}

{% tab title="Python (Requests)" %}
```python
import requests
import json

url = "https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_getBalance",
    "params": [
        "0x7a25d0f0Ff6a25f36De0a19C3d5f6dC2C6EAeD6a",
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
        "method": "eth_getBalance",
        "params": [
                "0x7a25d0f0Ff6a25f36De0a19C3d5f6dC2C6EAeD6a",
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
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x1bc16d674ec80000"
}
```
{% endcode %}

## Response Parameters

| Field  | Type     | Description                                      |
| ------ | -------- | ------------------------------------------------ |
| result | QUANTITY | Balance in wei (hex). Divide by 10^18 to get ETH |

## Use Cases

* Wallet balance display
* Pre-flight funds checks before submitting transactions
* Stock Token holder balance reads (Stock Tokens are ERC-20s — use `eth_call` on the token contract for token-specific balances)

## Error Handling

| Status Code | Error Message     | Cause                                          |
| ----------- | ----------------- | ---------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`            |
| -32602      | Invalid params    | Request parameters are missing or malformed    |
| -32603      | Internal error    | Server-side error while processing the request |
| 429         | Too Many Requests | Rate limit exceeded for your plan              |
| -32602      | Invalid address   | Address is not a valid 20-byte hex string      |

## SDK Integration

{% tabs %}
{% tab title="ethers.js" %}
{% code overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/');

// Generic JSON-RPC call (works for any method):
const result = await provider.send('eth_getBalance', ["0x7a25d0f0Ff6a25f36De0a19C3d5f6dC2C6EAeD6a", "latest"]);
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
    method: 'eth_getBalance',
    params: ["0x7a25d0f0Ff6a25f36De0a19C3d5f6dC2C6EAeD6a", "latest"]
});
console.log(result);

// Many standard methods have typed wrappers on PublicClient,
// e.g. client.getBalance({ address }), client.getBlock({ blockNumber }).
```
{% endtab %}
{% endtabs %}
