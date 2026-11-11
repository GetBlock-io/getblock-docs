---
description: >-
  Example code for the eth_getTransactionReceipt JSON-RPC method. Complete guide
  on how to use eth_getTransactionReceipt JSON-RPC in GetBlock Web3
  documentation.
---

# eth\_getTransactionReceipt - Harmony

This method returns the receipt of a mined transaction, including its status, gas used, and emitted logs. On Harmony the receipt also carries L1 fee fields. It returns null until the transaction is mined.

## Parameters

| Parameter       | Type   | Required | Description              |
| --------------- | ------ | -------- | ------------------------ |
| transactionHash | string | Yes      | 32-byte transaction hash |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getTransactionReceipt",
    "params": ["0x3c8a1f5b2d9e4076c1a8b3d5e7f9021436587a9cbdef012345678abcdef901234"],
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
    method: 'eth_getTransactionReceipt',
    params: ['0x3c8a1f5b2d9e4076c1a8b3d5e7f9021436587a9cbdef012345678abcdef901234'],
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
        'method': 'eth_getTransactionReceipt',
        'params': ['0x3c8a1f5b2d9e4076c1a8b3d5e7f9021436587a9cbdef012345678abcdef901234'],
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
            "method": "eth_getTransactionReceipt",
            "params": ["0x3c8a1f5b2d9e4076c1a8b3d5e7f9021436587a9cbdef012345678abcdef901234"],
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
    "result": {
        "transactionHash": "0x3c8a1f5b2d9e4076c1a8b3d5e7f9021436587a9cbdef012345678abcdef901234",
        "blockHash": "0x9f2b4c1d7e3a6084b5c9d0e1f2a3b4c5d6e7f8091a2b3c4d5e6f708192a3b4c5d",
        "blockNumber": "0x3d09000",
        "status": "0x1",
        "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
        "to": "0xcF664087a5bB0237a0BAd6742852ec6c8d69A27a",
        "gasUsed": "0x5208",
        "cumulativeGasUsed": "0x5208",
        "effectiveGasPrice": "0xf4240",
        "l1Fee": "0x2d79883d2000",
        "l1GasUsed": "0x640",
        "logs": []
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                                 |
| --------- | ------ | ----------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                           |
| id        | string | Request identifier matching the request                     |
| result    | object | Receipt object, or null if not yet mined (see fields below) |

### Result Object

| Field             | Type   | Description                                              |
| ----------------- | ------ | -------------------------------------------------------- |
| status            | string | 0x1 on success, 0x0 on revert                            |
| gasUsed           | string | L2 execution gas used by this transaction (hex)          |
| effectiveGasPrice | string | Actual L2 gas price paid (hex, wei)                      |
| l1Fee             | string | L1 data-availability fee for this transaction (hex, wei) |
| l1GasUsed         | string | L1 calldata gas attributed to this transaction (hex)     |
| logs              | array  | Event logs emitted during execution                      |

## Use Cases

* **Success Confirmation**: Read status to confirm a transaction succeeded
* **L2 Cost Accounting**: Sum L2 execution and L1 fees for true cost
* **Event Extraction**: Read logs to capture emitted events
* **Deployment Address**: Read contractAddress after a contract creation

## Error Handling

| Error Code | Message        | Description                         |
| ---------- | -------------- | ----------------------------------- |
| -32602     | Invalid params | Malformed transaction hash          |
| -32603     | Internal error | The node failed to read the receipt |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const receipt = await provider.getTransactionReceipt('0x3c8a1f5b2d9e4076c1a8b3d5e7f9021436587a9cbdef012345678abcdef901234');
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

const receipt = await client.getTransactionReceipt({ hash: '0x3c8a1f5b2d9e4076c1a8b3d5e7f9021436587a9cbdef012345678abcdef901234' });
```
{% endcode %}
{% endtab %}
{% endtabs %}
