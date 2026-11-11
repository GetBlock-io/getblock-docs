---
description: >-
  Example code for the getmempooldescendants JSON-RPC method. Complete guide on how to use
  getmempooldescendants JSON-RPC in GetBlock Web3 documentation.
---

# getmempooldescendants - Dogecoin

This method returns all in-mempool descendants of a transaction: the unconfirmed transactions that depend on it.

## Parameters

| Parameter | Type    | Required | Description                                                          |
| --------- | ------- | -------- | ---------------------------------------------------------------------- |
| txid      | string  | Yes      | The transaction id of a mempool entry                                |
| verbose   | boolean | No       | True for detailed objects keyed by txid, false for txids. Default false |

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
    "params": ["8d99a37f27d702b35bcc74dfea0d1a006a67c20571e1de660a30defc16316c8f", false],
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
  jsonrpc: '2.0', method: 'getmempooldescendants', params: ["8d99a37f27d702b35bcc74dfea0d1a006a67c20571e1de660a30defc16316c8f", false], id: 'getblock.io'
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
        'params': ["8d99a37f27d702b35bcc74dfea0d1a006a67c20571e1de660a30defc16316c8f", false],
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
            "params": ["8d99a37f27d702b35bcc74dfea0d1a006a67c20571e1de660a30defc16316c8f", false],
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
        "d0dd61c93cd4a571030c15b5f6a94466a8f7246b986dd318d656c910e6e5a8ed"
    ]
}
```

## Response Parameters

| Parameter | Type         | Description                                      |
| --------- | ------------ | ------------------------------------------------ |
| error     | null\|object | Error object when the call fails, otherwise null |
| id        | string       | Request identifier matching the request          |
| result | array|object | Descendant transaction ids, or objects keyed by txid when `verbose` is true |

## Use Cases

* **Impact Analysis**: See what else is blocked while a transaction is unconfirmed
* **Package Fees**: Total the descendant set when evaluating a fee bump
* **Chain Cleanup**: Find dependents before replacing a pending transaction

## Error Handling

| Error Code | Message | Description |
| ---------- | ------- | ----------- |
| -5 | Transaction not in mempool | The transaction id is not currently queued |
| -32700 | Parse error | Request body is not valid JSON |
| -32603 | Internal error | Node failed to process the request |
