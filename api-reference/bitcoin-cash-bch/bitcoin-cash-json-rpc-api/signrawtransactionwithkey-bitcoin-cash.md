---
description: >-
  Example code for the signrawtransactionwithkey JSON-RPC method. Complete guide
  on how to use the signrawtransactionwithkey JSON-RPC method in the GetBlock
  Web3 documentation.
---

# signrawtransactionwithkey - Bitcoin Cash

This method signs the inputs of a raw transaction using the supplied private keys. It returns the signed transaction hex and whether signing is complete.

## Parameters

| Parameter   | Type   | Required | Description                                  |
| ----------- | ------ | -------- | -------------------------------------------- |
| hexstring   | string | Yes      | The raw transaction hex to sign              |
| privkeys    | array  | Yes      | Array of base58 private keys to sign with    |
| prevtxs     | array  | No       | Previous output details required for signing |
| sighashtype | string | No       | The signature hash type. Default ALL\|FORKID |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "signrawtransactionwithkey",
    "params": ["01000000000101e17e03d21d051aa2bd9d336c3ac0693cfa92ce71592ceec521b1c48019ff77a101000000171600146d76e574b5f4825fe740ba6c41aaf1b319dfb80cffffffff02819a010000000000160014422002d927a1cae901eac668444cce8dd0ae60d529b31b0b0000000017a914f5b48d1130dc3d366d1eabf6783a552d1c8e08f487000000000", ["L1uyy5qTuGrVXrmrsvHWHgVzW9kKdrp27wBC7Vs6nZDTF2BRUVwy"], null, "ALL|FORKID"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="bitcoinjs-lib" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
// bitcoinjs-lib can sign locally; this RPC signs server-side with supplied keys.
const { data } = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
  jsonrpc: '2.0', method: 'signrawtransactionwithkey',
  params: [rawTxHex, [privKey], null, 'ALL|FORKID'], id: 'getblock.io'
});
console.log(data.result.complete);
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
        'method': 'signrawtransactionwithkey',
        'params': ["01000000000101e17e03d21d051aa2bd9d336c3ac0693cfa92ce71592ceec521b1c48019ff77a101000000171600146d76e574b5f48219ff77a101000000171600146d76e574b5f4825fe740ba6c41aaf1b319dfb80cffffffff02819a010000000000160014422002d927a1cae901eac668444cce8dd0ae60d529b31b0b0000000017a914f5b48d1130dc3d366d1eabf6783a552d1c8e08f487000000000", ["L1uyy5qTuGrVXrmrsvHWHgVzW9kKdrp27wBC7Vs6nZDTF2BRUVwy"], null, "ALL|FORKID"],
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
            "method": "signrawtransactionwithkey",
            "params": ["01000000000101e17e03d21d051aa2bd9d336c3ac0693cfa92ce71592ceec521b1c48019ff77a101000000171600146d76e574b5f4825fe740ba6c41aaf1b319dfb80cffffffff02819a010000000000160014422002d927a1cae901eac668444cce8dd0ae60d529b31b0b0000000017a914f5b48d1130dc3d366d1eabf6783a552d1c8e08f487000000000", ["L1uyy5qTuGrVXrmrsvHWHgVzW9kKdrp27wBC7Vs6nZDTF2BRUVwy"], null, "ALL|FORKID"],
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

| Parameter | Type         | Description                                                  |
| --------- | ------------ | ------------------------------------------------------------ |
| error     | null\|object | Error object when the call fails, otherwise null             |
| id        | string       | Request identifier matching the request                      |
| result    | object       | Object with the signed transaction hex and completion status |

### Result Object

| Field    | Type    | Description                                    |
| -------- | ------- | ---------------------------------------------- |
| hex      | string  | The signed transaction hex                     |
| complete | boolean | Whether all inputs are fully signed            |
| errors   | array   | Details of any inputs that could not be signed |

## Use Cases

* **Transaction Signing**: Sign a raw transaction with externally held keys
* **Multisig Flows**: Apply one signer's key as part of a multi-key process
* **Automated Payouts**: Sign transactions in a backend before broadcasting
* **Sighash Control**: Choose a signature hash type for specialized spends

## Error Handling

| Error Code | Message              | Description                                                     |
| ---------- | -------------------- | --------------------------------------------------------------- |
| -22        | TX decode failed     | The raw transaction hex could not be decoded                    |
| -26        | Transaction rejected | The transaction failed mempool acceptance checks                |
| -25        | Missing inputs       | One or more referenced inputs do not exist or are already spent |
| -32603     | Internal error       | Node failed to process the transaction                          |
