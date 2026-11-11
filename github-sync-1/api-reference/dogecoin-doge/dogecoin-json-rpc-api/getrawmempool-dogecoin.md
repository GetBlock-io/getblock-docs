---
description: >-
  Example code for the getrawmempool JSON-RPC method. Complete guide on how to use
  getrawmempool JSON-RPC in GetBlock Web3 documentation.
---

# getrawmempool - Dogecoin

This method returns the transaction ids of all transactions in the memory pool. Pass `verbose` as true to receive a detailed object for each entry instead.

## Parameters

| Parameter | Type    | Required | Description                                                          |
| --------- | ------- | -------- | ---------------------------------------------------------------------- |
| verbose   | boolean | No       | True for detailed objects keyed by txid, false for an array of txids. Default false |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getrawmempool",
    "params": [false],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="bitcoinjs-lib" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const { data } = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
  jsonrpc: '2.0', method: 'getrawmempool', params: [false], id: 'getblock.io'
});
console.log(data.result);
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
        'method': 'getrawmempool',
        'params': [false],
        'id': 'getblock.io'
    }
)

print(response.json()['result'])
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
            "method": "getrawmempool",
            "params": [false],
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
    "error": null,
    "id": "getblock.io",
    "result": [
        "d0dd61c93cd4a571030c15b5f6a94466a8f7246b986dd318d656c910e6e5a8ed",
        "8d99a37f27d702b35bcc74dfea0d1a006a67c20571e1de660a30defc16316c8f"
    ]
}
```

## Response Parameters

| Parameter | Type         | Description                                      |
| --------- | ------------ | ------------------------------------------------ |
| error     | null\|object | Error object when the call fails, otherwise null |
| id        | string       | Request identifier matching the request          |
| result | array|object | Transaction ids, or objects keyed by txid when `verbose` is true |

{% hint style="warning" %}
With `verbose` set to true the response grows with the size of the mempool and can be large during congestion. Prefer the default `false` form unless per-transaction detail is actually needed.
{% endhint %}

## Use Cases

* **Pending Detection**: See which transactions are waiting to be mined
* **Mempool Size**: Count entries to gauge congestion
* **Fee Analysis**: With `verbose`, read each entry's fee and size
* **Rebroadcast Checks**: Confirm a broadcast transaction is still queued

## Error Handling

| Error Code | Message | Description |
| ---------- | ------- | ----------- |
| -32700 | Parse error | Request body is not valid JSON |
| -32600 | Invalid request | The JSON sent is not a valid request object |
| -32603 | Internal error | Node failed to read the requested chain state |
