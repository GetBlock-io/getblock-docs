---
description: >-
  Example code for the eth_getTransactionByHash  JSON-RPC method. Complete guide
  to using the eth_getTransactionByHash  JSON-RPC method in GetBlock.io Web3
  documentation.
---

# eth\_getTransactionByHash - Robinhood

Returns transaction details by hash. Returns `null` if the transaction does not exist or is not yet mined (use `eth_getTransactionReceipt` for confirmed-only queries).

## Parameters

| Parameter | Type            | Required | Description      |
| --------- | --------------- | -------- | ---------------- |
| txHash    | DATA (32 bytes) | Yes      | Transaction hash |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByHash",
    "params": [
        "0xa8f9b3c7d2e4f6a1b9c8d7e6f5a4b3c2d1e0f9a8b7c6d5e4f3a2b1c0d9e8f7a6"
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
    "method": "eth_getTransactionByHash",
    "params": [
        "0xa8f9b3c7d2e4f6a1b9c8d7e6f5a4b3c2d1e0f9a8b7c6d5e4f3a2b1c0d9e8f7a6"
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

url = "hhttps://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByHash",
    "params": [
        "0xa8f9b3c7d2e4f6a1b9c8d7e6f5a4b3c2d1e0f9a8b7c6d5e4f3a2b1c0d9e8f7a6"
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
        "method": "eth_getTransactionByHash",
        "params": [
                "0xa8f9b3c7d2e4f6a1b9c8d7e6f5a4b3c2d1e0f9a8b7c6d5e4f3a2b1c0d9e8f7a6"
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
    "result": {
        "blockHash": "0x8b5c6f1a9e8d7c6b5a4f3e2d1c0b9a8e7f6d5c4b3a2b1f9e8d7c6b5a4f3e2d1c",
        "blockNumber": "0x124f80",
        "from": "0x7a25d0f0Ff6a25f36De0a19C3d5f6dC2C6EAeD6a",
        "to": "0x9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b",
        "gas": "0x5208",
        "gasPrice": "0x5f5e100",
        "maxFeePerGas": "0xba43b7400",
        "maxPriorityFeePerGas": "0x0",
        "hash": "0xa8f9b3c7d2e4f6a1b9c8d7e6f5a4b3c2d1e0f9a8b7c6d5e4f3a2b1c0d9e8f7a6",
        "input": "0xa9059cbb000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa9604500000000000000000000000000000000000000000000000016345785d8a00000",
        "nonce": "0x42",
        "value": "0x0",
        "type": "0x2",
        "chainId": "0x1237",
        "transactionIndex": "0x0",
        "v": "0x0",
        "r": "0x1b5e176d927f8e9ab405058b2d2457392da3e20f328b16ddabcebc33eaac5fea",
        "s": "0x3094a6d52d77b6dc9c4f74e58fb50a76b80e3a8b1f47b9c8a3e2f0d6c5b4a3e9"
    }
}
```
{% endcode %}

## Response Parameters

<table data-search="false"><thead><tr><th>Field</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td>result.hash</td><td>DATA</td><td>Transaction hash</td></tr><tr><td>result.from</td><td>DATA</td><td>Sender address</td></tr><tr><td>result.to</td><td>DATA</td><td>Recipient address; <code>null</code> for contract creation transactions</td></tr><tr><td>result.value</td><td>QUANTITY</td><td>ETH sent with the transaction in wei</td></tr><tr><td>result.gas</td><td>QUANTITY</td><td>Gas limit</td></tr><tr><td>result.gasPrice</td><td>QUANTITY</td><td>Gas price (legacy)</td></tr><tr><td>result.maxFeePerGas</td><td>QUANTITY</td><td>Max fee per gas (EIP-1559)</td></tr><tr><td>result.maxPriorityFeePerGas</td><td>QUANTITY</td><td>Max priority fee (EIP-1559)</td></tr><tr><td>result.input</td><td>DATA</td><td>Calldata</td></tr><tr><td>result.nonce</td><td>QUANTITY</td><td>Sender's transaction count at the time</td></tr><tr><td>result.blockHash</td><td>DATA</td><td>Block hash; <code>null</code> if not yet mined</td></tr><tr><td>result.blockNumber</td><td>QUANTITY</td><td>Block number; <code>null</code> if pending</td></tr><tr><td>result.chainId</td><td>QUANTITY</td><td>Chain ID — <code>0x1237</code> on Robinhood Chain mainnet</td></tr><tr><td>result.type</td><td>QUANTITY</td><td>Transaction type — <code>0x0</code> (legacy), <code>0x1</code> (EIP-2930), <code>0x2</code> (EIP-1559)</td></tr></tbody></table>

## Use Cases

* Reading transaction details after submission
* Wallet transaction history
* Replaying or reconstructing transactions

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
const result = await provider.send('eth_getTransactionByHash', ["0xa8f9b3c7d2e4f6a1b9c8d7e6f5a4b3c2d1e0f9a8b7c6d5e4f3a2b1c0d9e8f7a6"]);
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
    method: 'eth_getTransactionByHash',
    params: ["0xa8f9b3c7d2e4f6a1b9c8d7e6f5a4b3c2d1e0f9a8b7c6d5e4f3a2b1c0d9e8f7a6"]
});
console.log(result);

// Many standard methods have typed wrappers on PublicClient,
// e.g. client.getBalance({ address }), client.getBlock({ blockNumber }).
```
{% endtab %}
{% endtabs %}
