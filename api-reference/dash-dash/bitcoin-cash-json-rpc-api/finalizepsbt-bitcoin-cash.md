---
description: >-
  Example code for the finalizepsbt JSON-RPC method. Complete guide on how to
  use finalizepsbt JSON-RPC in GetBlock Web3 documentation.
---

# finalizepsbt - Bitcoin Cash

This method finalizes the inputs of a PSBT. If the transaction is fully signed it produces a network-ready hex; otherwise it returns the updated PSBT.

## Parameters

| Parameter | Type    | Required | Description                                                                                |
| --------- | ------- | -------- | ------------------------------------------------------------------------------------------ |
| psbt      | string  | Yes      | The base64-encoded PSBT to finalize                                                        |
| extract   | boolean | No       | If true and the PSBT is complete, extract and return the network transaction. Default true |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "finalizepsbt",
    "params": ["cHNidP8BAHECAAAAAeJ+A9IdBRqivZ0zbDrAaTz6ks5xWSzuxSGxxIAZ/3ehAQAAAAD/////AqGaAQAAAAAAFgAUQiAC2SehyukB6sZoREzOjdCuYNUps7ELAAAAABepFPW0jREw3D02bR6r9ng6VS0cjgj0hwAAAAAAAAAA", true],
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
  jsonrpc: '2.0', method: 'finalizepsbt',
  params: [psbt, true], id: 'getblock.io'
});
console.log(data.result.complete, data.result.hex);
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
        'method': 'finalizepsbt',
        'params': ["cHNidP8BAHECAAAAAeJ+A9IdBRqivZ0zbDrAaTz6ks5xWSzuxSGxxIAZ/3ehAQAAAAD/////AqGaAQAAAAAAFgAUQiAC2SehyukB6sZoREzOjdCuYNUps7ELAAAAABepFPW0jREw3D02bR6r9ng6VS0cjgj0hwAAAAAAAAAA", true],
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
            "method": "finalizepsbt",
            "params": ["cHNidP8BAHECAAAAAeJ+A9IdBRqivZ0zbDrAaTz6ks5xWSzuxSGxxIAZ/3ehAQAAAAD/////AqGaAQAAAAAAFgAUQiAC2SehyukB6sZoREzOjdCuYNUps7ELAAAAABepFPW0jREw3D02bR6r9ng6VS0cjgj0hwAAAAAAAAAA", true],
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
    "result": {
        "hex": "01000000000101e17e03d21d051aa2bd9d336c3ac0693cfa92ce71592ceec521b1c48019ff77a101000000171600146d76e574b5f4825fe740ba6c41aaf1b319dfb80cffffffff02819a010000000000160014422002d927a1cae901eac668444cce8dd0ae60d529b31b0b0000000017a914f5b48d1130dc3d366d1eabf6783a552d1c8e08f487000000000",
        "complete": true
    }
}
```

## Response Parameters

| Parameter | Type         | Description                                                     |
| --------- | ------------ | --------------------------------------------------------------- |
| error     | null\|object | Error object when the call fails, otherwise null                |
| id        | string       | Request identifier matching the request                         |
| result    | object       | Object with the finalized transaction hex and completion status |

### Result Object

| Field    | Type    | Description                                                                |
| -------- | ------- | -------------------------------------------------------------------------- |
| psbt     | string  | The finalized PSBT, present when extract is false or signing is incomplete |
| hex      | string  | The network-ready transaction hex, present when complete                   |
| complete | boolean | Whether all inputs are finalized                                           |

## Use Cases

* **Broadcast Prep**: Extract network-ready hex from a fully signed PSBT
* **Completion Checks**: Confirm all inputs are signed before broadcasting
* **Multisig Finish**: Finalize an m-of-n PSBT once enough signatures are gathered
* **Pipeline Handoff**: Produce hex for sendrawtransaction from a PSBT flow

## Error Handling

| Error Code | Message           | Description                                  |
| ---------- | ----------------- | -------------------------------------------- |
| -22        | Decode failed     | The supplied data could not be decoded       |
| -8         | Invalid parameter | A required parameter is missing or malformed |
| -32603     | Internal error    | Node failed to process the request           |
