---
description: >-
  Example code for the eth_newFilter JSON-RPC method. Complete guide to using
  the eth_newFilter JSON-RPC method in GetBlock.io Web3 documentation.
---

# eth\_newFilter - Robinhood

Creates a log filter object that can be polled for matching logs via `eth_getFilterChanges`. Filter persists on the node until removed by `eth_uninstallFilter` or expires from inactivity.

## Parameters

| Parameter | Type   | Required | Description                                                         |
| --------- | ------ | -------- | ------------------------------------------------------------------- |
| filter    | object | Yes      | Filter object: optional `fromBlock`, `toBlock`, `address`, `topics` |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_newFilter",
    "params": [
        {
            "address": "0x9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"
            ]
        }
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
    "method": "eth_newFilter",
    "params": [
        {
            "address": "0x9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"
            ]
        }
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
    "method": "eth_newFilter",
    "params": [
        {
            "address": "0x9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"
            ]
        }
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
{% code overflow="wrap" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let payload = json!({
        "jsonrpc": "2.0",
        "method": "eth_newFilter",
        "params": [
                {
                        "address": "0x9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b",
                        "topics": [
                                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"
                        ]
                }
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
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x1"
}
```
{% endcode %}

## Response Parameters

| Field  | Type     | Description                                |
| ------ | -------- | ------------------------------------------ |
| result | QUANTITY | Filter ID for use in subsequent poll calls |

## Use Cases

* Polling-based event tracking (alternative to WebSocket subscriptions)
* Legacy environments without WebSocket support

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
const result = await provider.send('eth_newFilter', [{"address": "0x9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b", "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]}]);
console.log(result);

// Many standard methods have typed wrappers on ethers Provider,
// e.g. provider.getBalance(addr), provider.getBlock(n), provider.getTransaction(hash).
```
{% endcode %}
{% endtab %}

{% tab title="viem" %}
{% code overflow="wrap" %}
```javascript
import { createPublicClient, http } from 'viem';

const client = createPublicClient({
    transport: http('https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/')
});

// Generic JSON-RPC request (works for any method):
const result = await client.request({
    method: 'eth_newFilter',
    params: [{"address": "0x9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b", "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]}]
});
console.log(result);

// Many standard methods have typed wrappers on PublicClient,
// e.g. client.getBalance({ address }), client.getBlock({ blockNumber }).
```
{% endcode %}
{% endtab %}
{% endtabs %}
