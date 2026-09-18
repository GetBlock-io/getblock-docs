---
description: >-
  Example code for the getblockheader JSON-RPC method. Complete guide on how to use
  getblockheader JSON-RPC in GetBlock Web3 documentation.
---

# getblockheader - Dogecoin

This method returns the header of a block, without the transactions it contains. Pass `verbose` as false to receive the serialized header as hex instead of an object.

## Parameters

| Parameter | Type    | Required | Description                                                     |
| --------- | ------- | -------- | --------------------------------------------------------------- |
| blockhash | string  | Yes      | The hash of the block whose header is returned                  |
| verbose   | boolean | No       | True for a JSON object, false for serialized hex. Default true  |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getblockheader",
    "params": ["afd10312739304ad5809a454bf723dd2f48fedc08600695fee147a23567be1cf", true],
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
  jsonrpc: '2.0', method: 'getblockheader', params: ["afd10312739304ad5809a454bf723dd2f48fedc08600695fee147a23567be1cf", true], id: 'getblock.io'
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
        'method': 'getblockheader',
        'params': ["afd10312739304ad5809a454bf723dd2f48fedc08600695fee147a23567be1cf", true],
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
            "method": "getblockheader",
            "params": ["afd10312739304ad5809a454bf723dd2f48fedc08600695fee147a23567be1cf", true],
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
        "hash": "afd10312739304ad5809a454bf723dd2f48fedc08600695fee147a23567be1cf",
        "confirmations": 43,
        "height": 6378930,
        "version": 6422788,
        "merkleroot": "3033dfe05f5377ca20680dbba85eefd14adbf932ddece22d06980538e8d14809",
        "time": 1789703885,
        "nonce": 0,
        "bits": "1967ad65",
        "difficulty": 41425662.4408129,
        "previousblockhash": "b1b6e89d831683c62b79af87de01df41caa6914fa977e5877027b14d99f0f8f6",
        "nextblockhash": "2ce761c3dff30841f1435165b3315ddf163243477d7bf1969f84e2608ac3a855"
    }
}
```

## Response Parameters

| Parameter | Type         | Description                                      |
| --------- | ------------ | ------------------------------------------------ |
| error     | null\|object | Error object when the call fails, otherwise null |
| id        | string       | Request identifier matching the request          |
| result.hash | string | The block hash |
| result.confirmations | numeric | Depth of the block, or -1 if not on the main chain |
| result.height | numeric | Block height |
| result.merkleroot | string | Merkle root of the block's transactions |
| result.time | numeric | Block time as a Unix timestamp |
| result.nonce | numeric | Block nonce. Always 0 on Dogecoin, which is merge-mined |
| result.bits | string | Compact difficulty target |
| result.previousblockhash | string | Hash of the preceding block |
| result.nextblockhash | string | Hash of the following block, when one exists |

{% hint style="info" %}
`nonce` is always `0` on Dogecoin. The chain is merge-mined with Litecoin, so the proof of work lives in an AuxPoW header rather than in this field. Do not treat a zero nonce as missing data.
{% endhint %}

## Use Cases

* **Light Verification**: Read a header without pulling the block's transactions
* **Chain Walking**: Follow `previousblockhash` to traverse backwards cheaply
* **Timestamp Checks**: Read a block's time without the payload cost
* **Confirmation Depth**: Read `confirmations` for a known block

## Error Handling

| Error Code | Message | Description |
| ---------- | ------- | ----------- |
| -32700 | Parse error | Request body is not valid JSON |
| -8 | Invalid parameter | A parameter is out of range or the wrong type |
| -32603 | Internal error | Node failed to process the request |
