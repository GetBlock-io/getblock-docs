---
description: >-
  Example code for the eth_getFilterChanges JSON-RPC method. Complete guide on
  how to use eth_getFilterChanges JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getFilterChanges - Moonbeam

This method returns the entries that have matched a filter since it was last polled: logs for a log filter, or hashes for block and pending-transaction filters.

## Parameters

| Parameter | Type   | Required | Description                                  |
| --------- | ------ | -------- | -------------------------------------------- |
| filterId  | string | Yes      | Filter ID returned by a create-filter method |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getFilterChanges",
    "params": ["0x1a4b0d21f8e0c3b6d2a9f7e4c1b8a5d2"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_getFilterChanges',
    params: ['0x1a4b0d21f8e0c3b6d2a9f7e4c1b8a5d2'],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests

response = requests.post(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'eth_getFilterChanges',
        'params': ['0x1a4b0d21f8e0c3b6d2a9f7e4c1b8a5d2'],
        'id': 'getblock.io'
    }
)

print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="example.rs" %}
```rust
use reqwest::Client;
use serde_json::{json, Value};

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let response = client
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "eth_getFilterChanges",
            "params": ["0x1a4b0d21f8e0c3b6d2a9f7e4c1b8a5d2"],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;

    println!("Result: {}", response["result"]);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": [
        {
            "address": "0xAcc15dC74880C9944775448304B263D191c6077F",
            "blockNumber": "0xb71b00",
            "transactionHash": "0x3c8a1f5b2d9e4076c1a8b3d5e7f9021436587a9cbdef012345678abcdef901234",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"
            ],
            "data": "0x"
        }
    ]
}
```

## Response Parameters

| Parameter | Type   | Description                                            |
| --------- | ------ | ------------------------------------------------------ |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                      |
| id        | string | Request identifier matching the request                |
| result    | array  | New logs or hashes since the last poll (empty if none) |

## Use Cases

* **Event Polling**: Fetch new logs for a log filter on an interval
* **Block Polling**: Fetch new block hashes for a block filter
* **Mempool Polling**: Fetch new pending hashes for a pending filter
* **Incremental Indexing**: Consume only entries since the previous poll

## Error Handling

| Error Code | Message          | Description                             |
| ---------- | ---------------- | --------------------------------------- |
| -32602     | Invalid params   | Malformed filter ID                     |
| -32000     | Filter not found | The filter expired or was never created |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

// ethers v6 delivers changes via event listeners rather than manual polling
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';

const client = createPublicClient({
  transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/')
});

const logs = await client.getFilterChanges({ filter });
```
{% endcode %}
{% endtab %}
{% endtabs %}
