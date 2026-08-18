---
description: >-
  Example code for the createrawtransaction JSON-RPC method. Complete guide on
  how to use createrawtransaction JSON-RPC in GetBlock Web3 documentation.
---

# createrawtransaction - Bitcoin Cash

This method creates an unsigned serialized transaction that spends the given inputs to the given outputs. The transaction is not stored or broadcast, only returned as hex.

## Parameters

| Parameter | Type    | Required | Description                                       |
| --------- | ------- | -------- | ------------------------------------------------- |
| inputs    | array   | Yes      | Array of input objects, each with txid and vout   |
| outputs   | array   | Yes      | Array of output objects mapping address to amount |
| locktime  | numeric | No       | Raw locktime for the transaction. Default 0       |

### Input Object

| Field | Type    | Required | Description                               |
| ----- | ------- | -------- | ----------------------------------------- |
| txid  | string  | Yes      | The transaction id of the output to spend |
| vout  | numeric | Yes      | The output index to spend                 |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "createrawtransaction",
    "params": [[{"txid": "10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642", "vout": 1}], [{"bitcoincash:qpm2qsznhks23z7629mms6s4cwef74vcwvy22gdx6a": 0.001}], 0],
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
  jsonrpc: '2.0', method: 'createrawtransaction',
  params: [[{ txid: '10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642', vout: 1 }], [{ 'bitcoincash:qpm2qsznhks23z7629mms6s4cwef74vcwvy22gdx6a': 0.001 }], 0],
  id: 'getblock.io'
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
        'method': 'createrawtransaction',
        'params': [[{"txid": "10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642", "vout": 1}], [{"bitcoincash:qpm2qsznhks23z7629mms6s4cwef74vcwvy22gdx6a": 0.001}], 0],
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
            "method": "createrawtransaction",
            "params": [[{"txid": "10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642", "vout": 1}], [{"bitcoincash:qpm2qsznhks23z7629mms6s4cwef74vcwvy22gdx6a": 0.001}], 0],
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

| Parameter | Type         | Description                                      |
| --------- | ------------ | ------------------------------------------------ |
| error     | null\|object | Error object when the call fails, otherwise null |
| id        | string       | Request identifier matching the request          |
| result    | string       | Hex-encoded unsigned raw transaction             |

## Use Cases

* **Manual Transactions**: Construct a spend with explicit input and output control
* **Coin Control**: Select specific UTXOs when building a transaction
* **Offline Signing**: Create an unsigned transaction to sign on an air-gapped device
* **Batch Construction**: Build a multi-output payment before signing

## Error Handling

| Error Code | Message           | Description                                  |
| ---------- | ----------------- | -------------------------------------------- |
| -22        | Decode failed     | The supplied data could not be decoded       |
| -8         | Invalid parameter | A required parameter is missing or malformed |
| -32603     | Internal error    | Node failed to process the request           |
