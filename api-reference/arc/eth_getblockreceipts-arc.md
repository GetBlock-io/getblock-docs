---
description: >-
  Example code for the eth_getBlockReceipts JSON-RPC method. Complete guide on
  how to use eth_getBlockReceipts JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getBlockReceipts - ARC

This method returns all transaction receipts for the transactions in a given block. It is significantly more efficient than calling `eth_getTransactionReceipt` for each transaction individually — essential for high-throughput indexers tracking Arc's payment flows.

## Parameters

| Parameter      | Type   | Required | Description                                   |
| -------------- | ------ | -------- | --------------------------------------------- |
| blockParameter | string | Yes      | Block number in hex, block tag, or block hash |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBlockReceipts",
    "params": [
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
    "method": "eth_getBlockReceipts",
    "params": [
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
    "method": "eth_getBlockReceipts",
    "params": [
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
        "method": "eth_getBlockReceipts",
        "params": [
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
    "result": [
        {
            "blockHash": "0x4f3a1d6e8c2b9a7e5d3f1c8a4b6e9d2f5a8c3e7b1d4f9a6c2e5b8d3f7a1c4e9b",
            "blockNumber": "0xa7d8c",
            "contractAddress": null,
            "cumulativeGasUsed": "0x5208",
            "effectiveGasPrice": "0x3b9aca00",
            "from": "0x4cef52a8f9d4b0c8b8d5f1a2b3c4d5e6f7a8b9c0",
            "gasUsed": "0x5208",
            "logs": [],
            "logsBloom": "0x0000000000000000000000000000000000000000000000000000000000000000",
            "status": "0x1",
            "to": "0xd85498dbeaeb1df24be52eed4f52eac2fbd56245",
            "transactionHash": "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b",
            "transactionIndex": "0x0",
            "type": "0x2"
        }
    ]
}
```
{% endcode %}

## Response Parameters

| Field                       | Type           | Description                                                |
| --------------------------- | -------------- | ---------------------------------------------------------- |
| result                      | array          | Array of receipt objects, one per transaction in the block |
| result\[].status            | string         | `0x1` for success, `0x0` for revert                        |
| result\[].gasUsed           | string         | Gas used by this transaction (hex)                         |
| result\[].effectiveGasPrice | string         | Effective gas price paid in USDC base units (hex)          |
| result\[].logs              | array          | Logs emitted by the transaction                            |
| result\[].contractAddress   | string \| null | Deployed contract address if this was a contract creation  |

## Use Cases

* Indexing payment outcomes for every transaction in a block in one call
* Building USDC-denominated gas-cost analytics per block
* Detecting reverted transactions and contract deployments
* Replacing N+1 receipt fetches with one batch call

## Error Handling

| Status Code | Error Message     | Cause                                       |
| ----------- | ----------------- | ------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`         |
| -32602      | Invalid params    | Request parameters are missing or malformed |
| -32601      | Method not found  | The method is not supported by this node    |
| 429         | Too Many Requests | Rate limit exceeded for your plan           |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// Most methods are exposed directly on the provider.
// For raw access, use provider.send(method, params).
const result = await provider.send('eth_getBlockReceipts', ["latest"]);
console.log(result);
```
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

const result = await client.request({ method: 'eth_getBlockReceipts', params: ["latest"] });
console.log(result);
```
{% endcode %}
{% endtab %}
{% endtabs %}
