# debug\_batchSendRawTransaction - Arbitrum Nova

This method submits an array of signed, serialized transactions in a single request and returns a result per transaction. It is a Dedicated Node tier convenience for batch broadcasting.

{% hint style="warning" %}
This method belongs to the `debug` namespace and is available on GetBlock **Dedicated Nodes** only. It is not served on shared endpoints.
{% endhint %}

## Parameters

| Parameter          | Type  | Required | Description                                          |
| ------------------ | ----- | -------- | ---------------------------------------------------- |
| signedTransactions | array | Yes      | Array of 0x-prefixed RLP-encoded signed transactions |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "debug_batchSendRawTransaction",
    "params": [["0x02f8b101...", "0x02f8b102..."]],
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
    method: 'debug_batchSendRawTransaction',
    params: [['0x02f8b101...', '0x02f8b102...']],
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
        'method': 'debug_batchSendRawTransaction',
        'params': [['0x02f8b101...', '0x02f8b102...']],
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
            "method": "debug_batchSendRawTransaction",
            "params": [["0x02f8b101...", "0x02f8b102..."]],
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
        "0x3c8a1f5b2d9e4076c1a8b3d5e7f9021436587a9cbdef012345678abcdef901234",
        "0x7ac91e2f4d0b3856a1c2f4e6d8b0a2c4e6f8091a3b5c7d9e1f3a5b7c9d1e3f507"
    ]
}
```

## Response Parameters

| Parameter | Type   | Description                                        |
| --------- | ------ | -------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                  |
| id        | string | Request identifier matching the request            |
| result    | array  | Per-transaction hash or error, in submission order |

## Use Cases

* **Batch Broadcast**: Submit many pre-signed transactions in one round trip
* **Load Testing**: Push transaction bursts to a test endpoint
* **Nonce Sequences**: Broadcast an ordered nonce sequence at once
* **Tooling**: Simplify multi-transaction submission in scripts

## Error Handling

| Error Code | Message          | Description                                         |
| ---------- | ---------------- | --------------------------------------------------- |
| -32602     | Invalid params   | One or more transactions are malformed              |
| -32601     | Method not found | The debug namespace is not enabled on this endpoint |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const results = await provider.send('debug_batchSendRawTransaction', [[signed1, signed2]]);
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

const results = await client.request({ method: 'debug_batchSendRawTransaction', params: [[signed1, signed2]] });
```
{% endcode %}
{% endtab %}
{% endtabs %}
