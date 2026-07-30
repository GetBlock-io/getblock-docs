---
description: >-
  Example code for the createpsbt JSON-RPC method. Complete guide on how to use
  createpsbt JSON-RPC in GetBlock Web3 documentation.
---

# createpsbt - Bitcoin Cash

This method creates an empty partially signed Bitcoin transaction (PSBT) with the given inputs and outputs and no signatures. The result is a base64-encoded PSBT.

## Parameters

| Parameter | Type    | Required | Description                                       |
| --------- | ------- | -------- | ------------------------------------------------- |
| inputs    | array   | Yes      | Array of input objects, each with txid and vout   |
| outputs   | array   | Yes      | Array of output objects mapping address to amount |
| locktime  | numeric | No       | Raw locktime. Default 0                           |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "createpsbt",
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
const { data } = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
  jsonrpc: '2.0', method: 'createpsbt',
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
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'createpsbt',
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
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "createpsbt",
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

{% code overflow="wrap" %}
```json
{
    "error": null,
    "id": "getblock.io",
    "result": "cHNidP8BAHECAAAAAeJ+A9IdBRqivZ0zbDrAaTz6ks5xWSzuxSGxxIAZ/3ehAQAAAAD/////AqGaAQAAAAAAFgAUQiAC2SehyukB6sZoREzOjdCuYNUps7ELAAAAABepFPW0jREw3D02bR6r9ng6VS0cjgj0hwAAAAAAAAAA"
}
```
{% endcode %}

## Response Parameters

| Parameter | Type         | Description                                      |
| --------- | ------------ | ------------------------------------------------ |
| error     | null\|object | Error object when the call fails, otherwise null |
| id        | string       | Request identifier matching the request          |
| result    | string       | Base64-encoded partially signed transaction      |

## Use Cases

* **Collaborative Signing**: Start a PSBT that multiple parties will sign in turn
* **Hardware Wallets**: Create a PSBT for a hardware device to sign offline
* **Coin Control**: Build a PSBT with explicitly chosen inputs
* **Multisig Setup**: Initialize a multisig spend for co-signers

## Error Handling

| Error Code | Message           | Description                                  |
| ---------- | ----------------- | -------------------------------------------- |
| -22        | Decode failed     | The supplied data could not be decoded       |
| -8         | Invalid parameter | A required parameter is missing or malformed |
| -32603     | Internal error    | Node failed to process the request           |
