---
description: >-
  Example code for the testmempoolaccept JSON-RPC method. Complete guide on how
  to use the testmempoolaccept JSON-RPC method in the GetBlock Web3
  documentation.
---

# testmempoolaccept - Bitcoin Cash

This method checks whether a raw transaction would be accepted into the mempool without actually submitting it. It is a dry run for broadcast.

## Parameters

| Parameter  | Type              | Required | Description                                                        |
| ---------- | ----------------- | -------- | ------------------------------------------------------------------ |
| rawtxs     | array             | Yes      | Array of raw transaction hex strings, currently limited to one     |
| maxfeerate | numeric or string | No       | Reject if fee rate exceeds this value, in BCH per kB. Default 0.10 |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "testmempoolaccept",
    "params": [["01000000000101e17e03d21d051aa2bd9d336c3ac0693cfa92ce71592ceec521b1c48019ff77a101000000171600146d76e574b5f4825fe740ba6c41aaf1b319dfb80cffffffff02819a010000000000160014422002d927a1cae901eac668444cce8dd0ae60d529b31b0b0000000017a914f5b48d1130dc3d366d1eabf6783a552d1c8e08f487000000000"], null],
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
  jsonrpc: '2.0', method: 'testmempoolaccept',
  params: [[rawTxHex], null], id: 'getblock.io'
});
console.log(data.result[0].allowed);
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
        'method': 'testmempoolaccept',
        'params': [["01000000000101e17e03d21d051aa2bd9d336c3ac0693cfa92ce71592ceec521b1c48019ff77a101000000171600146d76e574b5f4825fe740ba6c41aaf1b319dfb80cffffffff02819a010000000000160014422002d927a1cae901eac668444cce8dd0ae60d529b31b0b0000000017a914f5b48d1130dc3d366d1eabf6783a552d1c8e08f487000000000"], null],
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
            "method": "testmempoolaccept",
            "params": [["01000000000101e17e03d21d051aa2bd9d336c3ac0693cfa92ce71592ceec521b1c48019ff77a101000000171600146d76e574b5f4825fe740ba6c41aaf1b319dfb80cffffffff02819a010000000000160014422002d927a1cae901eac668444cce8dd0ae60d529b31b0b0000000017a914f5b48d1130dc3d366d1eabf6783a552d1c8e08f487000000000"], null],
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
        {
            "txid": "10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642",
            "allowed": true,
            "size": 223,
            "fees": {
                "base": 2.26e-06
            }
        }
    ]
}
```

## Response Parameters

| Parameter | Type         | Description                                                    |
| --------- | ------------ | -------------------------------------------------------------- |
| error     | null\|object | Error object when the call fails, otherwise null               |
| id        | string       | Request identifier matching the request                        |
| result    | array        | Array with one acceptance-test result per supplied transaction |

### Result Entry

| Field         | Type    | Description                                |
| ------------- | ------- | ------------------------------------------ |
| txid          | string  | The transaction id                         |
| allowed       | boolean | Whether the transaction would be accepted  |
| reject-reason | string  | Reason for rejection when allowed is false |
| fees          | object  | Fee details for the transaction            |

## Use Cases

* **Pre-Send Validation**: Confirm a transaction is relayable before broadcasting
* **Fee Checks**: Detect fee-rate rejections before paying to send
* **Policy Testing**: Verify a transaction meets mempool policy rules
* **Wallet UX**: Warn a user of rejection reasons before submission

## Error Handling

| Error Code | Message              | Description                                                     |
| ---------- | -------------------- | --------------------------------------------------------------- |
| -22        | TX decode failed     | The raw transaction hex could not be decoded                    |
| -26        | Transaction rejected | The transaction failed mempool acceptance checks                |
| -25        | Missing inputs       | One or more referenced inputs do not exist or are already spent |
| -32603     | Internal error       | Node failed to process the transaction                          |
