---
description: >-
  Example code for the sendrawtransaction JSON-RPC method. Complete guide on how
  to use the sendrawtransaction JSON-RPC method in the GetBlock Web3
  documentation.
---

# sendrawtransaction - Bitcoin Cash

This method submits a serialized, signed transaction to the network. The transaction is validated against mempool policy and, if accepted, relayed to peers.

## Parameters

| Parameter  | Type              | Required | Description                                                                                                 |
| ---------- | ----------------- | -------- | ----------------------------------------------------------------------------------------------------------- |
| hexstring  | string            | Yes      | The hex string of the raw signed transaction                                                                |
| maxfeerate | numeric or string | No       | Reject transactions with a fee rate above this value, in BCH per kB. Set 0 to accept any rate. Default 0.10 |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "sendrawtransaction",
    "params": ["01000000000101e17e03d21d051aa2bd9d336c3ac0693cfa92ce71592ceec521b1c48019ff77a101000000171600146d76e574b5f4825fe740ba6c41aaf1b319dfb80cffffffff02819a010000000000160014422002d927a1cae901eac668444cce8dd0ae60d529b31b0b0000000017a914f5b48d1130dc3d366d1eabf6783a552d1c8e08f487000000000", null],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="bitcoinjs-lib" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
// Build and sign with bitcoinjs-lib, then broadcast the raw hex through the node.
const { data } = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
  jsonrpc: '2.0', method: 'sendrawtransaction',
  params: [rawTxHex, null], id: 'getblock.io'
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
        'method': 'sendrawtransaction',
        'params': ["01000000000101e17e03d21d051aa2bd9d336c3ac0693cfa92ce71592ceec521b1c48019ff77a101000000171600146d76e574b5f4825fe740ba6c41aaf1b319dfb80cffffffff02819a010000000000160014422002d927a1cae901eac668444cce8dd0ae60d529b31b0b0000000017a914f5b48d1130dc3d366d1eabf6783a552d1c8e08f487000000000", null],
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
            "method": "sendrawtransaction",
            "params": ["01000000000101e17e03d21d051aa2bd9d336c3ac0693cfa92ce71592ceec521b1c48019ff77a101000000171600146d76e574b5f4825fe740ba6c41aaf1b319dfb80cffffffff02819a010000000000160014422002d927a1cae901eac668444cce8dd0ae60d529b31b0b0000000017a914f5b48d1130dc3d366d1eabf6783a552d1c8e08f487000000000", null],
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
    "result": "10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642"
}
```

## Response Parameters

| Parameter | Type         | Description                                      |
| --------- | ------------ | ------------------------------------------------ |
| error     | null\|object | Error object when the call fails, otherwise null |
| id        | string       | Request identifier matching the request          |
| result    | string       | The transaction id of the accepted transaction   |

## Use Cases

* **Payment Broadcast**: Submit a signed BCH payment to the network
* **Wallet Backends**: Relay transactions built and signed client-side
* **Fee Guards**: Use maxfeerate to reject accidentally overpaying transactions
* **Batch Payouts**: Broadcast a signed multi-output transaction

## Error Handling

| Error Code | Message              | Description                                                     |
| ---------- | -------------------- | --------------------------------------------------------------- |
| -22        | TX decode failed     | The raw transaction hex could not be decoded                    |
| -26        | Transaction rejected | The transaction failed mempool acceptance checks                |
| -25        | Missing inputs       | One or more referenced inputs do not exist or are already spent |
| -32603     | Internal error       | Node failed to process the transaction                          |
