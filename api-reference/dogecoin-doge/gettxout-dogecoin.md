---
description: >-
  Example code for the gettxout JSON-RPC method. Complete guide on how to use
  gettxout JSON-RPC in GetBlock Web3 documentation.
---

# gettxout - Dogecoin

This method returns details about an unspent transaction output (UTXO).

## Parameters

| Parameter      | Type    | Required | Description                                 |
| -------------- | ------- | -------- | ------------------------------------------- |
| txid           | string  | Yes      | The transaction ID.                         |
| n              | number  | Yes      | The vout index (output number).             |
| includemempool | boolean | No       | Whether to include mempool (default: true). |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "gettxout",
    "params": ["8e992e1512887da5e02fd52a1bd19d527ee54436934c215f0c23cf4ec04771e7", 0, true],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="gettxout.js" %}
```javascript
const axios = require('axios');

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "gettxout",
    "params": ["8e992e1512887da5e02fd52a1bd19d527ee54436934c215f0c23cf4ec04771e7", 0, true],
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
{% code title="gettxout.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "gettxout",
    "params": ["8e992e1512887da5e02fd52a1bd19d527ee54436934c215f0c23cf4ec04771e7", 0, True],
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
{% code title="gettxout.rs" %}
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
            "method": "gettxout",
            "params": ["8e992e1512887da5e02fd52a1bd19d527ee54436934c215f0c23cf4ec04771e7", 0, true],
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

{% code title="response.json" %}
```json
{
    "result": {
        "bestblock": "24f64485ea05250ce75a73b3549afad05e078a7d8abd54e695a957dadee264fa",
        "confirmations": 1389,
        "value": 10000.00000000,
        "scriptPubKey": {
            "asm": "OP_DUP OP_HASH160 a5f4d12ce3685781b227c1f39548ddef429e9783 OP_EQUALVERIFY OP_CHECKSIG",
            "hex": "76a914a5f4d12ce3685781b227c1f39548ddef429e978388ac",
            "reqSigs": 1,
            "type": "pubkeyhash",
            "addresses": [
                "DLGbK6mCjBT67r8wjJqCg8hkFiBYV5JquH"
            ]
        },
        "coinbase": true
    },
    "error": null,
    "id": "getblock.io"
}
```
{% endcode %}

## Response Parameters

| Field         | Type    | Description                                    |
| ------------- | ------- | ---------------------------------------------- |
| bestblock     | string  | The hash of the block at the tip of the chain. |
| confirmations | number  | Number of confirmations.                       |
| value         | number  | The value of the output in DOGE.               |
| scriptPubKey  | object  | The script details.                            |
| coinbase      | boolean | Whether this is a coinbase transaction output. |

## Use Case

The `gettxout` method is essential for:

* UTXO verification before spending
* Balance checking
* Transaction input validation
* Wallet UTXO management
* Double-spend detection

## Error Handling

| Error Code | Message   | Cause                            |
| ---------- | --------- | -------------------------------- |
| 403        | Forbidden | Missing or invalid ACCESS-TOKEN. |
