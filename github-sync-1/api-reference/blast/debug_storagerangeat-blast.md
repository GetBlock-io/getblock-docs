---
description: >-
  Example code for the debug_storageRangeAt JSON-RPC method. Complete guide on
  how to use debug_storageRangeAt JSON-RPC in GetBlock Web3 documentation.
---

# debug\_storageRangeAt - Blast

This method enumerates storage slots of a contract at a specific block and transaction index, starting from a hashed key. It is a Dedicated Node tier method for deep state inspection.

{% hint style="warning" %}
This method belongs to the `debug` namespace and is available on GetBlock **Dedicated Nodes** only. It is not served on shared endpoints.
{% endhint %}

## Parameters

| Parameter | Type   | Required | Description                        |
| --------- | ------ | -------- | ---------------------------------- |
| blockHash | string | Yes      | 32-byte block hash                 |
| txIndex   | number | Yes      | Transaction index within the block |
| address   | string | Yes      | 20-byte contract address           |
| startKey  | string | Yes      | Hashed storage key to start from   |
| limit     | number | Yes      | Maximum number of slots to return  |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "debug_storageRangeAt",
    "params": ["0x9f2b4c1d7e3a6084b5c9d0e1f2a3b4c5d6e7f8091a2b3c4d5e6f708192a3b4c5d", 0, "0x4200000000000000000000000000000000000006", "0x0000000000000000000000000000000000000000000000000000000000000000", 5],
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
    method: 'debug_storageRangeAt',
    params: ['0x9f2b4c1d7e3a6084b5c9d0e1f2a3b4c5d6e7f8091a2b3c4d5e6f708192a3b4c5d', 0, '0x4200000000000000000000000000000000000006', '0x0000000000000000000000000000000000000000000000000000000000000000', 5],
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
        'method': 'debug_storageRangeAt',
        'params': ['0x9f2b4c1d7e3a6084b5c9d0e1f2a3b4c5d6e7f8091a2b3c4d5e6f708192a3b4c5d', 0, '0x4200000000000000000000000000000000000006', '0x0000000000000000000000000000000000000000000000000000000000000000', 5],
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
            "method": "debug_storageRangeAt",
            "params": ["0x9f2b4c1d7e3a6084b5c9d0e1f2a3b4c5d6e7f8091a2b3c4d5e6f708192a3b4c5d", 0, "0x4200000000000000000000000000000000000006", "0x0000000000000000000000000000000000000000000000000000000000000000", 5],
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
        "storage": {},
        "nextKey": null
    }
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| id        | string | Request identifier matching the request |
| result    | object | Storage entries and a pagination cursor |

## Use Cases

* **Deep State Inspection**: Enumerate a contract's storage layout
* **Debugging**: Inspect storage as of a specific transaction index
* **Auditing**: Export contract storage for analysis
* **Reverse Engineering**: Map unknown storage slots to values

## Error Handling

| Error Code | Message          | Description                                         |
| ---------- | ---------------- | --------------------------------------------------- |
| -32602     | Invalid params   | Malformed block hash, index, address, or key        |
| -32601     | Method not found | The debug namespace is not enabled on this endpoint |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const range = await provider.send('debug_storageRangeAt', ['0x9f2b4c1d7e3a6084b5c9d0e1f2a3b4c5d6e7f8091a2b3c4d5e6f708192a3b4c5d', 0, '0x4200000000000000000000000000000000000006', '0x00...00', 5]);
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

const range = await client.request({ method: 'debug_storageRangeAt', params: ['0x9f2b4c1d7e3a6084b5c9d0e1f2a3b4c5d6e7f8091a2b3c4d5e6f708192a3b4c5d', 0, '0x4200000000000000000000000000000000000006', '0x00...00', 5] });
```
{% endcode %}
{% endtab %}
{% endtabs %}
