---
description: >-
  Example code for the base_transactionStatus Flashblocks method. Complete guide
  on how to use base_transactionStatus Flashblocks in GetBlock Web3
  documentation.
---

# base\_transactionStatus - Flashblocks

This Checks whether a specific transaction is currently present in the node's mempool. This confirms that a submitted transaction has been received by the sequencer _before_ it appears in a Flashblock — closing the gap between submission and first preconfirmation. Base-specific: this method exists on Base's Flashblocks endpoints but has no equivalent on Optimism.

{% hint style="warning" %}
**Base-only method.** This method is exposed by Base's Flashblocks infrastructure but has no equivalent on Optimism. Calling it against an Optimism endpoint will return `-32601 Method not found`.
{% endhint %}

## Parameters

| Parameter       | Type            | Required | Description                           |
| --------------- | --------------- | -------- | ------------------------------------- |
| transactionHash | DATA (32 bytes) | Yes      | The 32-byte transaction hash to query |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "base_transactionStatus",
    "params": [
        "0xa8f9b3c7d2e4f6a1b9c8d7e6f5a4b3c2d1e0f9a8b7c6d5e4f3a2b1c0d9e8f7a6"
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
    "method": "base_transactionStatus",
    "params": [
        "0xa8f9b3c7d2e4f6a1b9c8d7e6f5a4b3c2d1e0f9a8b7c6d5e4f3a2b1c0d9e8f7a6"
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
    "method": "base_transactionStatus",
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
        "method": "base_transactionStatus",
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
    "id": "getblock.io",
    "result": {
        "status": "Known"
    }
}
```
{% endcode %}

## Response Parameters

| Field         | Type   | Description                                                                                              |
| ------------- | ------ | -------------------------------------------------------------------------------------------------------- |
| result.status | string | `"Known"` if the transaction is present in the mempool; `"Unknown"` if it has not been seen by this node |

## Use Cases

* Confirming a submitted transaction reached the sequencer before it appears in a Flashblock
* Detecting transactions that were dropped or rejected before entering the mempool
* Debugging submission pipelines — distinguishing 'not submitted' from 'submitted but not yet preconfirmed'

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

// Flashblocks-specific methods use the raw send interface:
const result = await provider.send('base_transactionStatus', ["0xa8f9b3c7d2e4f6a1b9c8d7e6f5a4b3c2d1e0f9a8b7c6d5e4f3a2b1c0d9e8f7a6"]);
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

// Flashblocks-specific methods need the raw request transport:
const result = await client.request({
    method: 'base_transactionStatus',
    params: ["0xa8f9b3c7d2e4f6a1b9c8d7e6f5a4b3c2d1e0f9a8b7c6d5e4f3a2b1c0d9e8f7a6"]
});
console.log(result);
```
{% endtab %}
{% endtabs %}
