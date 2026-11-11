---
description: >-
  Example code for the eth_getBlockByNumber JSON-RPC method. Complete guide to
  using the eth_getBlockByNumber JSON-RPC method in GetBlock.io Web3
  documentation.
---

# eth\_getBlockByNumber - Robinhood

Returns block information by block number. Same response schema as `eth_getBlockByHash`. Use tags `latest`, `safe`, `finalized`, `pending`, `earliest` for relative block references. On Arbitrum Orbit, `finalized` means the block has settled on L1 (typically minutes behind `latest`).

## Parameters

| Parameter        | Type            | Required | Description                                                                      |
| ---------------- | --------------- | -------- | -------------------------------------------------------------------------------- |
| blockNumber      | QUANTITY \| TAG | Yes      | Block number (hex) or tag (`latest`, `safe`, `finalized`, `pending`, `earliest`) |
| fullTransactions | boolean         | Yes      | `true` for full transaction objects, `false` for hashes only                     |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBlockByNumber",
    "params": [
        "latest",
        false
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
    "method": "eth_getBlockByNumber",
    "params": [
        "latest",
        false
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
    "method": "eth_getBlockByNumber",
    "params": [
        "latest",
        false
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
        "method": "eth_getBlockByNumber",
        "params": [
                "latest",
                false
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

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "number": "0x124f80",
        "hash": "0x8b5c6f1a9e8d7c6b5a4f3e2d1c0b9a8e7f6d5c4b3a2b1f9e8d7c6b5a4f3e2d1c",
        "parentHash": "0x47edbdc8da5cd1b3127ea8f1ce5da6739fa39e7e6d2eb1c9b50f1c83de0a98b4",
        "timestamp": "0x68f3b85f",
        "miner": "0xa4b000000000000000000073657175656e636572",
        "gasLimit": "0x1c9c380",
        "gasUsed": "0xe4e1c0",
        "baseFeePerGas": "0x5f5e100",
        "l1BlockNumber": "0x1234567",
        "transactions": [
            "0xa8f9b3c7d2e4f6a1b9c8d7e6f5a4b3c2d1e0f9a8b7c6d5e4f3a2b1c0d9e8f7a6"
        ]
    }
}
```

## Response Parameters

| Field  | Type          | Description                                               |
| ------ | ------------- | --------------------------------------------------------- |
| result | Block \| null | Block object, or `null` if no block at this number exists |

## Use Cases

* Sequential block scanning by height
* Indexer backfill jobs
* Block explorer pages
* Distinguishing `latest` (soft-confirmed) vs `finalized` (L1-settled) blocks

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
const result = await provider.send('eth_getBlockByNumber', ["latest", false]);
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
    method: 'eth_getBlockByNumber',
    params: ["latest", false]
});
console.log(result);

// Many standard methods have typed wrappers on PublicClient,
// e.g. client.getBalance({ address }), client.getBlock({ blockNumber }).
```
{% endtab %}
{% endtabs %}
