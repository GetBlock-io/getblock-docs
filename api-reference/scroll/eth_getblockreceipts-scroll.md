---
description: >-
  Example code for the eth_getBlockReceipts JSON-RPC method. Complete guide on
  how to use eth_getBlockReceipts JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getBlockReceipts scroll

This method returns every transaction receipt in a block in a single call. It is used to index all logs and statuses for a block without one request per transaction.

## Parameters

| Parameter      | Type   | Required | Description                                                       |
| -------------- | ------ | -------- | ----------------------------------------------------------------- |
| blockParameter | string | Yes      | Block number in hex, a block hash, or "latest"/"safe"/"finalized" |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBlockReceipts",
    "params": ["latest"],
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
    method: 'eth_getBlockReceipts',
    params: ['latest'],
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
        'method': 'eth_getBlockReceipts',
        'params': ['latest'],
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
            "method": "eth_getBlockReceipts",
            "params": ["latest"],
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
            "transactionHash": "0x3c8a1f5b2d9e4076c1a8b3d5e7f9021436587a9cbdef012345678abcdef901234",
            "blockNumber": "0x14b8a10",
            "status": "0x1",
            "gasUsed": "0x5208",
            "logs": []
        }
    ]
}
```

## Response Parameters

| Parameter | Type   | Description                                                |
| --------- | ------ | ---------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                          |
| id        | string | Request identifier matching the request                    |
| result    | array  | Array of receipt objects, one per transaction in the block |

## Use Cases

* **Efficient Indexing**: Pull every receipt for a block in one round trip
* **Log Aggregation**: Collect all event logs emitted in a block
* **Status Sweeps**: Check success/failure of all transactions in a block
* **Explorer Backends**: Populate a block page with per-transaction outcomes

## Error Handling

| Error Code | Message        | Description                              |
| ---------- | -------------- | ---------------------------------------- |
| -32602     | Invalid params | Malformed block number, hash, or tag     |
| -32603     | Internal error | The node failed to assemble the receipts |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const receipts = await provider.send('eth_getBlockReceipts', ['latest']);
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

const receipts = await client.request({ method: 'eth_getBlockReceipts', params: ['latest'] });
```
{% endcode %}
{% endtab %}
{% endtabs %}
