---
description: >-
  Example code for the getrawmempool JSON-RPC method. Complete guide on how to
  use the getrawmempool JSON-RPC method in the GetBlock Web3 documentation.
---

# getrawmempool - Bitcoin Cash

This method returns the transaction ids currently in the node's mempool. With verbose set to true it returns detailed mempool entry data keyed by txid.

## Parameters

| Parameter | Type    | Required | Description                                                           |
| --------- | ------- | -------- | --------------------------------------------------------------------- |
| verbose   | boolean | No       | true for detailed objects, false for an array of txids. Default false |

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
console.log(data.result.length);
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
        "10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642",
        "780791bb2d5a8ccda4b5a707967a8e15b412814852c58c77299e85579bb65587"
    ]
}
```

## Response Parameters

| Parameter | Type          | Description                                                          |
| --------- | ------------- | -------------------------------------------------------------------- |
| error     | null\|object  | Error object when the call fails, otherwise null                     |
| id        | string        | Request identifier matching the request                              |
| result    | array\|object | Array of txids, or an object of mempool entries when verbose is true |

## Use Cases

* **Pending Monitoring**: Watch unconfirmed transactions as they enter the mempool
* **Fee Market Analysis**: Inspect verbose entries to model current fee pressure
* **Payment Detection**: Detect an incoming transaction before it confirms
* **Congestion Signals**: Track mempool size to gauge network load

## Error Handling

| Error Code | Message         | Description                                   |
| ---------- | --------------- | --------------------------------------------- |
| -32700     | Parse error     | Request body is not valid JSON                |
| -32600     | Invalid request | The JSON sent is not a valid request object   |
| -32603     | Internal error  | Node failed to read the requested chain state |
