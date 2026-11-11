---
description: >-
  Example code for the eth_getTransactionReceipt  JSON-RPC method. Complete
  guide to using the eth_getTransactionReceipt  JSON-RPC method in GetBlock.io
  Web3 documentation.
---

# eth\_getTransactionReceipt - Robinhood

Returns the receipt of a confirmed transaction — gas used, logs emitted, status (1 = success, 0 = revert), and contract address if the transaction created a contract. Essential for confirming Stock Token transfers, DeFi operations, and any state-changing transaction.

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
    "method": "eth_getTransactionReceipt",
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
    "method": "eth_getTransactionReceipt",
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

url = "https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_getTransactionReceipt",
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
        "method": "eth_getTransactionReceipt",
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
        "contractAddress": null,
        "cumulativeGasUsed": "0x5208",
        "effectiveGasPrice": "0x5f5e100",
        "from": "0x7a25d0f0Ff6a25f36De0a19C3d5f6dC2C6EAeD6a",
        "gasUsed": "0x5208",
        "logs": [
            {
                "address": "0x9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b",
                "blockHash": "0x8b5c6f1a9e8d7c6b5a4f3e2d1c0b9a8e7f6d5c4b3a2b1f9e8d7c6b5a4f3e2d1c",
                "blockNumber": "0x124f80",
                "data": "0x000000000000000000000000000000000000000000000000016345785d8a0000",
                "logIndex": "0x0",
                "removed": false,
                "topics": [
                    "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                    "0x0000000000000000000000007a25d0f0Ff6a25f36De0a19C3d5f6dC2C6EAeD6a",
                    "0x0000000000000000000000009c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b"
                ],
                "transactionHash": "0xa8f9b3c7d2e4f6a1b9c8d7e6f5a4b3c2d1e0f9a8b7c6d5e4f3a2b1c0d9e8f7a6",
                "transactionIndex": "0x0"
            }
        ],
        "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
        "status": "0x1",
        "to": "0x9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b",
        "transactionHash": "0xa8f9b3c7d2e4f6a1b9c8d7e6f5a4b3c2d1e0f9a8b7c6d5e4f3a2b1c0d9e8f7a6",
        "transactionIndex": "0x0",
        "type": "0x2"
    }
}
```
{% endcode %}

## Response Parameters

<table data-search="false"><thead><tr><th>Field</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td>result.status</td><td>QUANTITY</td><td><code>0x1</code> = success, <code>0x0</code> = reverted</td></tr><tr><td>result.gasUsed</td><td>QUANTITY</td><td>Actual gas consumed by this transaction</td></tr><tr><td>result.effectiveGasPrice</td><td>QUANTITY</td><td>Effective gas price paid (for EIP-1559 calculations)</td></tr><tr><td>result.logs</td><td>array of Log</td><td>Event logs emitted during execution</td></tr><tr><td>result.contractAddress</td><td>DATA</td><td>Address of newly created contract, or <code>null</code> for normal transactions</td></tr><tr><td>result.from</td><td>DATA</td><td>Sender address</td></tr><tr><td>result.to</td><td>DATA</td><td>Recipient address</td></tr><tr><td>result.cumulativeGasUsed</td><td>QUANTITY</td><td>Total gas used in the block up to and including this transaction</td></tr></tbody></table>

## Use Cases

* Verifying transaction success or failure after submission
* Reading Stock Token Transfer event logs
* Computing actual transaction cost (gasUsed × effectiveGasPrice)
* Detecting newly deployed contract addresses

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
const result = await provider.send('eth_getTransactionReceipt', ["0xa8f9b3c7d2e4f6a1b9c8d7e6f5a4b3c2d1e0f9a8b7c6d5e4f3a2b1c0d9e8f7a6"]);
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
    method: 'eth_getTransactionReceipt',
    params: ["0xa8f9b3c7d2e4f6a1b9c8d7e6f5a4b3c2d1e0f9a8b7c6d5e4f3a2b1c0d9e8f7a6"]
});
console.log(result);

// Many standard methods have typed wrappers on PublicClient,
// e.g. client.getBalance({ address }), client.getBlock({ blockNumber }).
```
{% endtab %}
{% endtabs %}
