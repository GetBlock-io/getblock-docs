---
description: >-
  Example code for the eth_getLogs JSON-RPC method. Complete guide to using the
  eth_getLogs JSON-RPC method in GetBlock.io Web3 documentation.
---

# eth\_getLogs - Robinhood

Returns event logs matching the given filter criteria — by address, topics, and block range. The primary way to read on-chain events, including Stock Token Transfer events and DeFi protocol events.

## Parameters

| Parameter | Type   | Required | Description                                                                          |
| --------- | ------ | -------- | ------------------------------------------------------------------------------------ |
| filter    | object | Yes      | Filter object with optional `fromBlock`, `toBlock`, `address`, `topics`, `blockHash` |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getLogs",
    "params": [
        {
            "fromBlock": "0x124e00",
            "toBlock": "latest",
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
    "method": "eth_getLogs",
    "params": [
        {
            "fromBlock": "0x124e00",
            "toBlock": "latest",
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
    "method": "eth_getLogs",
    "params": [
        {
            "fromBlock": "0x124e00",
            "toBlock": "latest",
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
        "method": "eth_getLogs",
        "params": [
                {
                        "fromBlock": "0x124e00",
                        "toBlock": "latest",
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

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": [
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
    ]
}
```

## Response Parameters

| Field                     | Type          | Description                                     |
| ------------------------- | ------------- | ----------------------------------------------- |
| result                    | array of Log  | Matching log entries                            |
| result\[].address         | DATA          | Address that emitted the log                    |
| result\[].topics          | array of DATA | Indexed event topics (first is event signature) |
| result\[].data            | DATA          | Non-indexed event data                          |
| result\[].blockNumber     | QUANTITY      | Block number containing the log                 |
| result\[].transactionHash | DATA          | Hash of the transaction that emitted the log    |

## Use Cases

* Reading Stock Token Transfer events (canonical token tracking)
* Indexer event scanning for any contract activity
* Building activity feeds for accounts (filter by indexed address topic)

## Error Handling

| Status Code | Error Message     | Cause                                                              |
| ----------- | ----------------- | ------------------------------------------------------------------ |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                                |
| -32602      | Invalid params    | Request parameters are missing or malformed                        |
| -32603      | Internal error    | Server-side error while processing the request                     |
| 429         | Too Many Requests | Rate limit exceeded for your plan                                  |
| -32602      | Invalid filter    | Block range too large, or filter object malformed                  |
| -32005      | Limit exceeded    | Too many logs returned — narrow the block range or add more topics |

## SDK Integration

{% tabs %}
{% tab title="ethers.js" %}
{% code overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/');

// Generic JSON-RPC call (works for any method):
const result = await provider.send('eth_getLogs', [{"fromBlock": "0x124e00", "toBlock": "latest", "address": "0x9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b", "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]}]);
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
    method: 'eth_getLogs',
    params: [{"fromBlock": "0x124e00", "toBlock": "latest", "address": "0x9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b", "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]}]
});
console.log(result);

// Many standard methods have typed wrappers on PublicClient,
// e.g. client.getBalance({ address }), client.getBlock({ blockNumber }).
```
{% endtab %}
{% endtabs %}
