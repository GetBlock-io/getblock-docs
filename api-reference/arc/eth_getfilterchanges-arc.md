---
description: >-
  Example code for the eth_getFilterChanges JSON-RPC method. Complete guide on
  how to use eth_getFilterChanges JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getFilterChanges - ARC

This method polls a filter (created with `eth_newFilter`, `eth_newBlockFilter`, or `eth_newPendingTransactionFilter`) and returns any events that have occurred since the previous poll.

## Parameters

| Parameter | Type   | Required | Description                                                                                          |
| --------- | ------ | -------- | ---------------------------------------------------------------------------------------------------- |
| filterId  | string | Yes      | The filter ID returned by `eth_newFilter` / `eth_newBlockFilter` / `eth_newPendingTransactionFilter` |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getFilterChanges",
    "params": [
        "0x16f1f5f5dc91c4a1d2e7e8b6c4d3f2e1"
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
    "method": "eth_getFilterChanges",
    "params": [
        "0x16f1f5f5dc91c4a1d2e7e8b6c4d3f2e1"
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
    "method": "eth_getFilterChanges",
    "params": [
        "0x16f1f5f5dc91c4a1d2e7e8b6c4d3f2e1"
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
        "method": "eth_getFilterChanges",
        "params": [
                "0x16f1f5f5dc91c4a1d2e7e8b6c4d3f2e1"
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
            "address": "0x4cef52a8f9d4b0c8b8d5f1a2b3c4d5e6f7a8b9c0",
            "blockHash": "0x4f3a1d6e8c2b9a7e5d3f1c8a4b6e9d2f5a8c3e7b1d4f9a6c2e5b8d3f7a1c4e9b",
            "blockNumber": "0xa7d8c",
            "data": "0x0000000000000000000000000000000000000000000000000000000005f5e100",
            "logIndex": "0x0",
            "removed": false,
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"
            ],
            "transactionHash": "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b",
            "transactionIndex": "0x0"
        }
    ]
}
```
{% endcode %}

## Response Parameters

| Field  | Type  | Description                                                                                                                                       |
| ------ | ----- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| result | array | Array of new events since the last poll — either log objects (log filter), block hashes (block filter), or transaction hashes (pending tx filter) |

## Use Cases

* Polling loops for server-side event subscribers
* Bridging HTTP-only environments to event-driven architectures
* Lightweight subscriber pattern without WebSocket overhead

## Error Handling

| Status Code | Error Message     | Cause                                                 |
| ----------- | ----------------- | ----------------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                   |
| -32602      | Invalid params    | Request parameters are missing or malformed           |
| -32601      | Method not found  | The method is not supported by this node              |
| 429         | Too Many Requests | Rate limit exceeded for your plan                     |
| -32000      | Filter not found  | Filter has expired or never existed; create a new one |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// Most methods are exposed directly on the provider.
// For raw access, use provider.send(method, params).
const result = await provider.send('eth_getFilterChanges', ["0x16f1f5f5dc91c4a1d2e7e8b6c4d3f2e1"]);
console.log(result);
```
{% endtab %}

{% tab title="Viem" %}
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

const result = await client.request({ method: 'eth_getFilterChanges', params: ["0x16f1f5f5dc91c4a1d2e7e8b6c4d3f2e1"] });
console.log(result);
```
{% endtab %}
{% endtabs %}
