---
description: >-
  Example code for the getrawtransaction JSON-RPC method. Complete guide on how
  to use getrawtransaction JSON-RPC in GetBlock Web3 documentation.
---

# getrawtransaction - Dogecoin

This method returns the raw transaction data for a given transaction ID.

## Parameters

| Parameter | Type   | Required | Description                                                    |
| --------- | ------ | -------- | -------------------------------------------------------------- |
| txid      | string | Yes      | The transaction ID.                                            |
| verbose   | number | No       | If 0 (default), returns hex string. If 1, returns JSON object. |

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getrawtransaction",
    "params": ["8e992e1512887da5e02fd52a1bd19d527ee54436934c215f0c23cf4ec04771e7", 1],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
{% code title="getrawtransaction.js" %}
```javascript
const axios = require('axios');

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "getrawtransaction",
    "params": ["8e992e1512887da5e02fd52a1bd19d527ee54436934c215f0c23cf4ec04771e7", 1],
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
{% code title="getrawtransaction.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "getrawtransaction",
    "params": ["8e992e1512887da5e02fd52a1bd19d527ee54436934c215f0c23cf4ec04771e7", 1],
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
{% code title="getrawtransaction.rs" %}
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
            "method": "getrawtransaction",
            "params": ["8e992e1512887da5e02fd52a1bd19d527ee54436934c215f0c23cf4ec04771e7", 1],
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

## Response (verbose=1)

```json
{
    "result": {
        "txid": "8e992e1512887da5e02fd52a1bd19d527ee54436934c215f0c23cf4ec04771e7",
        "hash": "8e992e1512887da5e02fd52a1bd19d527ee54436934c215f0c23cf4ec04771e7",
        "version": 1,
        "size": 190,
        "vsize": 190,
        "locktime": 0,
        "vin": [
            {
                "coinbase": "03549120...",
                "sequence": 4294967295
            }
        ],
        "vout": [
            {
                "value": 10000.00000000,
                "n": 0,
                "scriptPubKey": {
                    "asm": "OP_DUP OP_HASH160 ... OP_EQUALVERIFY OP_CHECKSIG",
                    "hex": "76a914...88ac",
                    "reqSigs": 1,
                    "type": "pubkeyhash",
                    "addresses": [
                        "DLGbK6mCjBT67r8wjJqCg8hkFiBYV5JquH"
                    ]
                }
            }
        ],
        "blockhash": "24f64485ea05250ce75a73b3549afad05e078a7d8abd54e695a957dadee264fa",
        "confirmations": 1389,
        "time": 1632136777,
        "blocktime": 1632136777
    },
    "error": null,
    "id": "getblock.io"
}
```

## Response Parameters

| Field         | Type   | Description                            |
| ------------- | ------ | -------------------------------------- |
| txid          | string | The transaction ID.                    |
| hash          | string | The transaction hash.                  |
| version       | number | Transaction version.                   |
| size          | number | Transaction size in bytes.             |
| vsize         | number | Virtual transaction size.              |
| locktime      | number | Lock time.                             |
| vin           | array  | Array of transaction inputs.           |
| vout          | array  | Array of transaction outputs.          |
| blockhash     | string | Block hash containing the transaction. |
| confirmations | number | Number of confirmations.               |
| time          | number | Transaction time.                      |
| blocktime     | number | Block time.                            |

## Use Case

The `getrawtransaction` method is essential for:

* Transaction lookup and verification
* Building block explorers
* Payment confirmation systems
* Transaction analysis tools
* Wallet transaction history

## Error Handling

| Error Code | Message                              | Cause                                      |
| ---------- | ------------------------------------ | ------------------------------------------ |
| -5         | Invalid or non-wallet transaction id | Transaction not found or txindex disabled. |
| 403        | Forbidden                            | Missing or invalid ACCESS-TOKEN.           |
| -8         | invalid param                        |                                            |
