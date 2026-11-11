---
description: >-
  Example code for the getrawtransaction JSON-RPC method. Complete guide on how
  to use the getrawtransaction JSON-RPC method in the GetBlock Web3
  documentation.
---

# getrawtransaction - Bitcoin Cash

This method returns the raw transaction data for a transaction id. With verbose set to false it returns the serialized hex; with true it returns a decoded JSON object.

## Parameters

| Parameter | Type    | Required | Description                                             |
| --------- | ------- | -------- | ------------------------------------------------------- |
| txid      | string  | Yes      | The transaction id                                      |
| verbose   | boolean | No       | false for hex, true for a decoded object. Default false |
| blockhash | string  | No       | The block in which to look for the transaction          |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getrawtransaction",
    "params": ["10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642", false, null],
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
  jsonrpc: '2.0', method: 'getrawtransaction',
  params: ['10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642', false, null], id: 'getblock.io'
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
        'method': 'getrawtransaction',
        'params': ["10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642", false, null],
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
            "method": "getrawtransaction",
            "params": ["10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642", false, null],
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
    "result": "01000000000101e17e03d21d051aa2bd9d336c3ac0693cfa92ce71592ceec521b1c48019ff77a101000000171600146d76e574b5f4825fe740ba6c41aaf1b319dfb80cffffffff02819a010000000000160014422002d927a1cae901eac668444cce8dd0ae60d529b31b0b0000000017a914f5b48d1130dc3d366d1eabf6783a552d1c8e08f487000000000"
}
```

## Response Parameters

| Parameter | Type           | Description                                                          |
| --------- | -------------- | -------------------------------------------------------------------- |
| error     | null\|object   | Error object when the call fails, otherwise null                     |
| id        | string         | Request identifier matching the request                              |
| result    | string\|object | Serialized transaction hex, or a decoded object when verbose is true |

## Use Cases

* **Transaction Decoding**: Retrieve and inspect a confirmed transaction's inputs and outputs
* **Receipt Rendering**: Display transaction detail in a wallet or explorer
* **Proof Construction**: Fetch raw hex needed to build or verify a transaction proof
* **Rebroadcast**: Retrieve raw hex to resubmit a dropped transaction

## Error Handling

| Error Code | Message               | Description                                                              |
| ---------- | --------------------- | ------------------------------------------------------------------------ |
| -8         | Invalid parameter     | txid is not a valid 64-character hex string                              |
| -5         | Transaction not found | No transaction with the given id is available; a txindex may be required |
| -32603     | Internal error        | Node failed to read the transaction                                      |
