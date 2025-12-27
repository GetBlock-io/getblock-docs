---
description: >-
  Example code for the signrawtransaction JSON-RPC method. Complete guide on how
  to use signrawtransaction JSON-RPC in GetBlock Web3 documentation.
---

# signrawtransaction - Dogecoin

This method signs inputs for a raw transaction (serialized, hex-encoded).

{% hint style="info" %}
Notes:

* Private keys must be provided if wallet doesn't contain them.
* Returns `complete: false` if more signatures are needed.
* Sighash types: ALL, NONE, SINGLE (with optional ANYONECANPAY).
{% endhint %}

## Parameters

| Parameter   | Type   | Required | Description                            |
| ----------- | ------ | -------- | -------------------------------------- |
| hexstring   | string | Yes      | The hex-encoded raw transaction.       |
| prevtxs     | array  | No       | Array of previous transaction outputs. |
| privkeys    | array  | No       | Array of private keys for signing.     |
| sighashtype | string | No       | Signature hash type (default: ALL).    |

### Previous Transaction Object Structure

```json
{
    "txid": "transaction_id",
    "vout": 0,
    "scriptPubKey": "hex_script",
    "redeemScript": "hex_script"
}
```

## Request

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "signrawtransaction",
    "params": [
        "0100000001e77147c04ecf230c5f214c933644e57e529dd11b2ad52fe0a57d8812152e998e0000000000ffffffff0100e40b54020000001976a914a5f4d12ce3685781b227c1f39548ddef429e978388ac00000000",
        [{"txid": "8e992e1512887da5e02fd52a1bd19d527ee54436934c215f0c23cf4ec04771e7", "vout": 0, "scriptPubKey": "76a914...88ac"}],
        null
    ],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const axios = require('axios');

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "signrawtransaction",
    "params": [
        "0100000001e77147c04ecf230c5f214c933644e57e529dd11b2ad52fe0a57d8812152e998e0000000000ffffffff0100e40b54020000001976a914a5f4d12ce3685781b227c1f39548ddef429e978388ac00000000",
        [{"txid": "8e992e1512887da5e02fd52a1bd19d527ee54436934c215f0c23cf4ec04771e7", "vout": 0, "scriptPubKey": "76a914...88ac"}],
        null
    ],
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
{% code title="example.py" overflow="wrap" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "signrawtransaction",
    "params": [
        "0100000001e77147c04ecf230c5f214c933644e57e529dd11b2ad52fe0a57d8812152e998e0000000000ffffffff0100e40b54020000001976a914a5f4d12ce3685781b227c1f39548ddef429e978388ac00000000",
        [{"txid": "8e992e1512887da5e02fd52a1bd19d527ee54436934c215f0c23cf4ec04771e7", "vout": 0, "scriptPubKey": "76a914...88ac"}],
        None
    ],
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
{% code title="main.rs" %}
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
            "method": "signrawtransaction",
            "params": [
                "0100000001...",
                null,
                null
            ],
            "id": "getblock.io"
        }"#)
        .send()
        .await?;
    
    println!("{}", response.text().await?);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "result": {
        "hex": "0100000001e77147c04ecf230c5f214c933644e57e529dd11b2ad52fe0a57d8812152e998e000000006a47304402...",
        "complete": true
    },
    "error": null,
    "id": "getblock.io"
}
```

## Response Parameters

| Field    | Type    | Description                                      |
| -------- | ------- | ------------------------------------------------ |
| hex      | string  | The signed raw transaction (hex-encoded).        |
| complete | boolean | True if transaction has all required signatures. |

## Use Case

The `signrawtransaction` method is essential for:

* Signing transactions before broadcast
* Multi-signature transaction workflows
* Offline transaction signing
* Hardware wallet integration
* Cold storage operations

## Error Handling

| Error Code | Message           | Cause                               |
| ---------- | ----------------- | ----------------------------------- |
| -22        | TX decode failed  | Invalid raw transaction hex.        |
| -8         | Invalid parameter | Invalid prevtxs or privkeys format. |
| 403        | Forbidden         | Missing or invalid ACCESS-TOKEN.    |
