---
description: >-
  Example code for the decoderawtransaction JSON-RPC method. Complete guide on
  how to use decoderawtransaction JSON-RPC in GetBlock Web3 documentation.
---

# decoderawtransaction - Bitcoin Cash

This method decodes a serialized transaction hex string into a JSON object describing its inputs, outputs, and metadata. It does not require the transaction to exist on-chain.

## Parameters

| Parameter | Type   | Required | Description                          |
| --------- | ------ | -------- | ------------------------------------ |
| hexstring | string | Yes      | The transaction hex string to decode |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "decoderawtransaction",
    "params": ["01000000000101e17e03d21d051aa2bd9d336c3ac0693cfa92ce71592ceec521b1c48019ff77a101000000171600146d76e574b5f4825fe740ba6c41aaf1b319dfb80cffffffff02819a010000000000160014422002d927a1cae901eac668444cce8dd0ae60d529b31b0b0000000017a914f5b48d1130dc3d366d1eabf6783a552d1c8e08f487000000000"],
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
  jsonrpc: '2.0', method: 'decoderawtransaction',
  params: [rawTxHex], id: 'getblock.io'
});
console.log(data.result.txid, data.result.vout);
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
        'method': 'decoderawtransaction',
        'params': ["01000000000101e17e03d21d051aa2bd9d336c3ac0693cfa92ce71592ceec521b1c48019ff77a101000000171600146d76e574b5f4825fe740ba6c41aaf1b319dfb80cffffffff02819a010000000000160014422002d927a1cae901eac668444cce8dd0ae60d529b31b0b0000000017a914f5b48d1130dc3d366d1eabf6783a552d1c8e08f487000000000"],
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
            "method": "decoderawtransaction",
            "params": ["01000000000101e17e03d21d051aa2bd9d336c3ac0693cfa92ce71592ceec521b1c48019ff77a101000000171600146d76e574b5f4825fe740ba6c41aaf1b319dfb80cffffffff02819a010000000000160014422002d927a1cae901eac668444cce8dd0ae60d529b31b0b0000000017a914f5b48d1130dc3d366d1eabf6783a552d1c8e08f487000000000"],
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
        "txid": "10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642",
        "hash": "10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642",
        "version": 1,
        "size": 223,
        "locktime": 0,
        "vin": [
            {
                "txid": "780791bb2d5a8ccda4b5a707967a8e15b412814852c58c77299e85579bb65587",
                "vout": 1,
                "scriptSig": {
                    "asm": "",
                    "hex": ""
                },
                "sequence": 4294967295
            }
        ],
        "vout": [
            {
                "value": 0.00105345,
                "n": 0,
                "scriptPubKey": {
                    "asm": "OP_HASH160 f5b48d... OP_EQUAL",
                    "hex": "a914f5b48d...87",
                    "reqSigs": 1,
                    "type": "scripthash",
                    "addresses": [
                        "bitcoincash:ppm2qsznhks23z7629mms6s4cwef74vcwvn0h829pq"
                    ]
                }
            }
        ]
    }
}
```

## Response Parameters

| Parameter | Type         | Description                                      |
| --------- | ------------ | ------------------------------------------------ |
| error     | null\|object | Error object when the call fails, otherwise null |
| id        | string       | Request identifier matching the request          |
| result    | object       | Decoded transaction object                       |

### Result Object

| Field    | Type    | Description                  |
| -------- | ------- | ---------------------------- |
| txid     | string  | The transaction id           |
| version  | numeric | Transaction version          |
| locktime | numeric | Transaction locktime         |
| vin      | array   | Array of transaction inputs  |
| vout     | array   | Array of transaction outputs |

## Use Cases

* **Pre-Broadcast Review**: Inspect a transaction's outputs before signing or sending
* **Debugging**: Verify a manually constructed transaction is well-formed
* **Address Extraction**: Read destination addresses from an unsigned transaction
* **Fee Verification**: Confirm output amounts before committing to a spend

## Error Handling

| Error Code | Message           | Description                                  |
| ---------- | ----------------- | -------------------------------------------- |
| -22        | Decode failed     | The supplied data could not be decoded       |
| -8         | Invalid parameter | A required parameter is missing or malformed |
| -32603     | Internal error    | Node failed to process the request           |
