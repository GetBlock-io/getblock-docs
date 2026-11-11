---
description: >-
  Example code for the gettxoutproof JSON-RPC method. Complete guide on how to use
  gettxoutproof JSON-RPC in GetBlock Web3 documentation.
---

# gettxoutproof - Dogecoin

This method returns a hex-encoded merkle proof that the given transactions were included in a block. The proof can be verified independently with `verifytxoutproof`.

## Parameters

| Parameter | Type   | Required | Description                                                               |
| --------- | ------ | -------- | --------------------------------------------------------------------------- |
| txids     | array  | Yes      | Transaction ids to prove. All must be in the same block                   |
| blockhash | string | No       | Look for the transactions in this block instead of searching the UTXO set |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "gettxoutproof",
    "params": [["d0dd61c93cd4a571030c15b5f6a94466a8f7246b986dd318d656c910e6e5a8ed"], "afd10312739304ad5809a454bf723dd2f48fedc08600695fee147a23567be1cf"],
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
  jsonrpc: '2.0', method: 'gettxoutproof', params: [["d0dd61c93cd4a571030c15b5f6a94466a8f7246b986dd318d656c910e6e5a8ed"], "afd10312739304ad5809a454bf723dd2f48fedc08600695fee147a23567be1cf"], id: 'getblock.io'
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
        'method': 'gettxoutproof',
        'params': [["d0dd61c93cd4a571030c15b5f6a94466a8f7246b986dd318d656c910e6e5a8ed"], "afd10312739304ad5809a454bf723dd2f48fedc08600695fee147a23567be1cf"],
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
            "method": "gettxoutproof",
            "params": [["d0dd61c93cd4a571030c15b5f6a94466a8f7246b986dd318d656c910e6e5a8ed"], "afd10312739304ad5809a454bf723dd2f48fedc08600695fee147a23567be1cf"],
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
    "result": "04006200b1b6e89d831683c62b79af87de01df41caa6914fa977e5877027b14d99f0f8f6..."
}
```

## Response Parameters

| Parameter | Type         | Description                                      |
| --------- | ------------ | ------------------------------------------------ |
| error     | null\|object | Error object when the call fails, otherwise null |
| id        | string       | Request identifier matching the request          |
| result | string | The hex-encoded merkle proof |

{% hint style="info" %}
Supplying `blockhash` is strongly preferred. Without it the node has to locate the transaction through the UTXO set, which fails for any output that has already been spent.
{% endhint %}

## Use Cases

* **SPV Verification**: Prove inclusion to a client that holds only headers
* **Audit Trails**: Store a compact proof alongside a payment record
* **Cross-System Attestation**: Hand a counterparty verifiable inclusion evidence

## Error Handling

| Error Code | Message | Description |
| ---------- | ------- | ----------- |
| -5 | Transaction not yet in block | The transaction is unconfirmed or not found |
| -8 | Not all transactions found in specified or retrieved block | The txids do not share the given block |
| -32603 | Internal error | Node failed to build the proof |
