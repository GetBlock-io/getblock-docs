---
description: >-
  Example code for the getmempooldescendants JSON-RPC method. Complete guide on
  how to use the getmempooldescendants JSON-RPC method in the GetBlock Web3
  documentation.
---

# getmempooldescendants - Bitcoin Cash

This method returns the in-mempool descendants of a transaction: the unconfirmed transactions that depend on it. Verbose mode returns full entry data for each descendant.

## Parameters

| Parameter | Type    | Required | Description                                                           |
| --------- | ------- | -------- | --------------------------------------------------------------------- |
| txid      | string  | Yes      | The transaction id, which must be in the mempool                      |
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
    "method": "getmempooldescendants",
    "params": ["10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642", false],
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
  jsonrpc: '2.0', method: 'getmempooldescendants',
  params: ['10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642', false], id: 'getblock.io'
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
        'method': 'getmempooldescendants',
        'params': ["10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642", false],
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
            "method": "getmempooldescendants",
            "params": ["10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642", false],
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
        "780791bb2d5a8ccda4b5a707967a8e15b412814852c58c77299e85579bb65587"
    ]
}
```

## Response Parameters

| Parameter | Type          | Description                                                         |
| --------- | ------------- | ------------------------------------------------------------------- |
| error     | null\|object  | Error object when the call fails, otherwise null                    |
| id        | string        | Request identifier matching the request                             |
| result    | array\|object | Array of descendant txids, or detailed objects when verbose is true |

## Use Cases

* **Impact Analysis**: See which unconfirmed transactions depend on a given one
* **Eviction Risk**: Assess how many descendants would be affected by an eviction
* **Fee Bumping**: Account for descendants when computing effective fee rates
* **Chain Limits**: Check descendant counts against mempool chain limits

## Error Handling

| Error Code | Message               | Description                                                              |
| ---------- | --------------------- | ------------------------------------------------------------------------ |
| -8         | Invalid parameter     | txid is not a valid 64-character hex string                              |
| -5         | Transaction not found | No transaction with the given id is available; a txindex may be required |
| -32603     | Internal error        | Node failed to read the transaction                                      |
