---
description: >-
  Example code for the eth_getFilterLogs JSON-RPC method. Сomplete guide on how
  to use eth_getFilterLogs JSON-RPC in GetBlock.io Web3 documentation.
---

# eth\_getFilterLogs - opBNB

This method returns all logs matching a filter, regardless of when the filter was created. Unlike `eth_getFilterChanges`, this returns the full set of matches rather than just new events.

## Parameters

| Parameter | Type   | Required | Description                               |
| --------- | ------ | -------- | ----------------------------------------- |
| filterId  | string | Yes      | The filter ID returned by `eth_newFilter` |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getFilterLogs",
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
    "method": "eth_getFilterLogs",
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
    "method": "eth_getFilterLogs",
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
        "method": "eth_getFilterLogs",
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
            "address": "0x9e7c5e3e3a3b8e1aa0e2d4c7f9d4b0c8b8d5f1a2",
            "blockHash": "0x4f3a1d6e8c2b9a7e5d3f1c8a4b6e9d2f5a8c3e7b1d4f9a6c2e5b8d3f7a1c4e9b",
            "blockNumber": "0x2d4e8a3",
            "data": "0x0000000000000000000000000000000000000000000000056bc75e2d63100000",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"
            ],
            "transactionHash": "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"
        }
    ]
}
```
{% endcode %}

## Response Parameters

| Field  | Type  | Description                           |
| ------ | ----- | ------------------------------------- |
| result | array | Array of all logs matching the filter |

## Use Cases

* Reading the full event history for a filter without recreating it
* Backfilling state after process restart

## Error Handling

| Status Code | Error Message     | Cause                                       |
| ----------- | ----------------- | ------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`         |
| -32602      | Invalid params    | Request parameters are missing or malformed |
| -32601      | Method not found  | The method is not supported by this node    |
| 429         | Too Many Requests | Rate limit exceeded for your plan           |
| -32000      | Filter not found  | Filter has expired or never existed         |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// Most methods are exposed directly on the provider.
// For raw access, use provider.send(method, params).
const result = await provider.send('eth_getFilterLogs', ["0x16f1f5f5dc91c4a1d2e7e8b6c4d3f2e1"]);
console.log(result);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { opBNB } from 'viem/chains';

const client = createPublicClient({
    chain: opBNB,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const result = await client.request({ method: 'eth_getFilterLogs', params: ["0x16f1f5f5dc91c4a1d2e7e8b6c4d3f2e1"] });
console.log(result);
```
{% endtab %}
{% endtabs %}
