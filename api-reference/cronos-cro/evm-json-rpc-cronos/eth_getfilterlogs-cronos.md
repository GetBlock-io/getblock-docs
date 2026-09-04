---
description: >-
  Example code for the eth_getFilterLogs JSON-RPC method. Complete guide on how
  to use eth_getFilterLogs JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getFilterLogs - Cronos

This method returns all logs matching a previously created log filter, regardless of when they were last polled. It is used to backfill the full history of a filter.

## Parameters

| Parameter | Type   | Required | Description                          |
| --------- | ------ | -------- | ------------------------------------ |
| filterId  | string | Yes      | Filter ID returned by eth\_newFilter |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getFilterLogs",
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
    method: 'eth_getFilterLogs',
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
        'method': 'eth_getFilterLogs',
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
            "method": "eth_getFilterLogs",
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
            "address": "0x5C7F8A570d578ED84E63fdFA7b1eE72dEae1AE23",
            "blockNumber": "0x1900000",
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

| Parameter | Type   | Description                                       |
| --------- | ------ | ------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                 |
| id        | string | Request identifier matching the request           |
| result    | array  | All logs matching the filter over its block range |

## Use Cases

* **History Backfill**: Load all matching logs for a filter's range at once
* **Re-Sync**: Rebuild an event index after a restart
* **Audit Queries**: Pull the complete event set for a contract range
* **Verification**: Cross-check incremental polling against the full log set

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

const logs = await provider.getLogs({ address: '0x5C7F8A570d578ED84E63fdFA7b1eE72dEae1AE23', topics: ['0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef'] });
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

const logs = await client.getFilterLogs({ filter });
```
{% endcode %}
{% endtab %}
{% endtabs %}
