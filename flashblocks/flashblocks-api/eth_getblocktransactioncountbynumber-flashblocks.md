---
description: >-
  Example code for eth_getBlockTransactionCountByNumber Flashblocks method.
  Complete guide on how to use eth_getBlockTransactionCountByNumber Flashblocks
  in GetBlock Web3 documentation.
---

# eth\_getBlockTransactionCountByNumber - Flashblocks

This returns the pending number of transactions in a block.&#x20;

## Parameters

| Parameter    | Type     | Required | Description             |
| ------------ | -------- | -------- | ----------------------- |
| block number | QUANTITY | TAG, Yes | the pending transaction |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
  "jsonrpc": "2.0",
  "method": "eth_getBlockTransactionCountByNumber",
  "params": [
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
  "method": "eth_getBlockTransactionCountByNumber",
  "params": [
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
  "method": "eth_getBlockTransactionCountByNumber",
  "params": [
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
        "method": "eth_getTransactionByHash",
        "params": [
                "0xa8f9b3c7d2e4f6a1b9c8d7e6f5a4b3c2d1e0f9a8b7c6d5e4f3a2b1c0d9e8f7a6"
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
    "result": "0x8b",
    "id": "getblock.io"
}

```
{% endcode %}

## Response Parameters

| Field  | Type   | Description                                            |
| ------ | ------ | ------------------------------------------------------ |
| result | string | `0x00...00` for preconfirmed; real hash once finalized |

## Use Cases

* Confirming a transaction has been received and included in a Flashblock
* Reading calldata / value of a preconfirmed transaction for follow-up processing
* Building activity feeds with sub-second latency

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

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// Generic JSON-RPC call — 'pending' returns Flashblocks-preconfirmed state:
const result = await provider.send('eth_getBlockTransactionCountByNumber', ["pending"]);
console.log(result);
```
{% endcode %}
{% endtab %}

{% tab title="viem" %}
```javascript
import { createPublicClient, http } from 'viem';

const client = createPublicClient({
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

// viem's read methods accept blockTag: 'pending' for Flashblocks-preconfirmed state:
const result = await client.request({
    method: 'eth_getBlockTransactionCountByNumber',
    params: ["pending"]
});
console.log(result);
```
{% endtab %}
{% endtabs %}
