---
description: >-
  Example code for the decoderawtransaction JSON-RPC method. Complete guide on
  how to use decoderawtransaction JSON-RPC in GetBlock Web3 documentation.
---

# decoderawtransaction - Dogecoin

This method decodes a hex-encoded raw transaction and returns a JSON object representing it.

## Parameters

| Parameter | Type   | Required | Description                      |
| --------- | ------ | -------- | -------------------------------- |
| hexstring | string | Yes      | The hex-encoded raw transaction. |

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
    "params": ["01000000013324faf8f03695261611669f5bdd93b68a86cf02db12dc46642ed50666ed69b8000000006a47304402200e1bf722d4335179de170f7c762755b463b3f7b8f026f30950f701bc834f0e6e022036295fdd5e607ca41c4e0e62e59d0911b607bfabedde2424665ffae13564d0e001210388f8f226d12eccd3ba93c1454ec4498b065cea96e29b918fbdb517872ebbf581ffffffff0200a5459b010000001976a91418a89ee36293f15c4db4c01173babd579243161188ac60b8c4b8000000001976a914c6977da37560e1432c2e14e16952981a4c272cac88ac00000000"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code overflow="wrap" %}
```javascript
const axios = require('axios');

const rawTx = "01000000013324faf8f03695261611669f5bdd93b68a86cf02db12dc46642ed50666ed69b8000000006a47304402200e1bf722d4335179de170f7c762755b463b3f7b8f026f30950f701bc834f0e6e022036295fdd5e607ca41c4e0e62e59d0911b607bfabedde2424665ffae13564d0e001210388f8f226d12eccd3ba93c1454ec4498b065cea96e29b918fbdb517872ebbf581ffffffff0200a5459b010000001976a91418a89ee36293f15c4db4c01173babd579243161188ac60b8c4b8000000001976a914c6977da37560e1432c2e14e16952981a4c272cac88ac00000000";

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "decoderawtransaction",
    "params": [rawTx],
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers: {
        'Content-Type': 'application/json'
    },
    data: data
};

axios(config)
    .then(response => console.log(JSON.stringify(response.data)))
    .catch(error => console.log(error));
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code overflow="wrap" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

raw_tx = "01000000013324faf8f03695261611669f5bdd93b68a86cf02db12dc46642ed50666ed69b8000000006a47304402200e1bf722d4335179de170f7c762755b463b3f7b8f026f30950f701bc834f0e6e022036295fdd5e607ca41c4e0e62e59d0911b607bfabedde2424665ffae13564d0e001210388f8f226d12eccd3ba93c1454ec4498b065cea96e29b918fbdb517872ebbf581ffffffff0200a5459b010000001976a91418a89ee36293f15c4db4c01173babd579243161188ac60b8c4b8000000001976a914c6977da37560e1432c2e14e16952981a4c272cac88ac00000000"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "decoderawtransaction",
    "params": [raw_tx],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
```rust
use reqwest::header;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = reqwest::Client::new();
    
    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header(header::CONTENT_TYPE, "application/json")
        .body(r#"{
            "jsonrpc": "2.0",
            "method": "decoderawtransaction",
            "params": ["01000000013324faf8..."],
            "id": "getblock.io"
        }"#)
        .send()
        .await?;
    
    println!("{}", response.text().await?);
    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Response

{% code title="response.json" overflow="wrap" %}
```json
{
    "result": {
        "txid": "b4fae2a43cb35f8016a547e9658e061f1da4a043efafecc42f739d46d95dee21",
        "hash": "b4fae2a43cb35f8016a547e9658e061f1da4a043efafecc42f739d46d95dee21",
        "version": 1,
        "size": 225,
        "vsize": 225,
        "locktime": 0,
        "vin": [
            {
                "txid": "b869ed6606d52e6446dc12db02cf868ab693dd5b9f66111626953695f8fa2433",
                "vout": 0,
                "scriptSig": {
                    "asm": "304402200e1bf722d4335179de170f7c762755b463b3f7b8f026f30950f701bc834f0e6e022036295fdd5e607ca41c4e0e62e59d0911b607bfabedde2424665ffae13564d0e0[ALL] 0388f8f226d12eccd3ba93c1454ec4498b065cea96e29b918fbdb517872ebbf581",
                    "hex": "47304402200e1bf722d4335179de170f7c762755b463b3f7b8f026f30950f701bc834f0e6e022036295fdd5e607ca41c4e0e62e59d0911b607bfabedde2424665ffae13564d0e001210388f8f226d12eccd3ba93c1454ec4498b065cea96e29b918fbdb517872ebbf581"
                },
                "sequence": 4294967295
            }
        ],
        "vout": [
            {
                "value": 69.00000000,
                "n": 0,
                "scriptPubKey": {
                    "asm": "OP_DUP OP_HASH160 18a89ee36293f15c4db4c01173babd5792431611 OP_EQUALVERIFY OP_CHECKSIG",
                    "hex": "76a91418a89ee36293f15c4db4c01173babd579243161188ac",
                    "reqSigs": 1,
                    "type": "pubkeyhash",
                    "addresses": [
                        "D5yJKJgQw4gvYj3g2oEMhYTe2x3K3qPLhS"
                    ]
                }
            },
            {
                "value": 31.00000000,
                "n": 1,
                "scriptPubKey": {
                    "asm": "OP_DUP OP_HASH160 c6977da37560e1432c2e14e16952981a4c272cac OP_EQUALVERIFY OP_CHECKSIG",
                    "hex": "76a914c6977da37560e1432c2e14e16952981a4c272cac88ac",
                    "reqSigs": 1,
                    "type": "pubkeyhash",
                    "addresses": [
                        "DLGFqLk5A8jLGhpPAwVvPSzGnspFEPK8o6"
                    ]
                }
            }
        ]
    },
    "error": null,
    "id": "getblock.io"
}
```
{% endcode %}

## Response Parameters

| Field    | Type   | Description                |
| -------- | ------ | -------------------------- |
| txid     | string | The transaction ID.        |
| hash     | string | The transaction hash.      |
| version  | number | Transaction version.       |
| size     | number | Transaction size in bytes. |
| vsize    | number | Virtual transaction size.  |
| locktime | number | Transaction lock time.     |
| vin      | array  | Array of input objects.    |
| vout     | array  | Array of output objects.   |

## Use Case

The `decoderawtransaction` method is essential for:

* Transaction inspection before broadcast
* Transaction verification
* Debugging transaction issues
* Block explorer functionality
* Wallet transaction parsing

## Error Handling

| Error Code | Message          | Cause                                        |
| ---------- | ---------------- | -------------------------------------------- |
| -22        | TX decode failed | Invalid hex string or malformed transaction. |
| 403        | Forbidden        | Missing or invalid ACCESS-TOKEN.             |
