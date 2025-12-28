---
description: >-
  Example code for the gettransaction JSON-RPC method. Complete guide on how to
  use gettransaction JSON-RPC in GetBlock Web3 documentation.
---

# gettransaction - Dogecoin {disallowed}

This method returns detailed information about a transaction.

## Parameters

| Parameter | Type   | Required | Description         |
| --------- | ------ | -------- | ------------------- |
| txid      | string | Yes      | The transaction ID. |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "gettransaction",
    "params": ["8e992e1512887da5e02fd52a1bd19d527ee54436934c215f0c23cf4ec04771e7"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="gettransaction.js" %}
```javascript
const axios = require('axios');

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "gettransaction",
    "params": ["8e992e1512887da5e02fd52a1bd19d527ee54436934c215f0c23cf4ec04771e7"],
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
{% code title="gettransaction.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "gettransaction",
    "params": ["8e992e1512887da5e02fd52a1bd19d527ee54436934c215f0c23cf4ec04771e7"],
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
            "method": "gettransaction",
            "params": ["8e992e1512887da5e02fd52a1bd19d527ee54436934c215f0c23cf4ec04771e7"],
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

{% code title="Response (example).json" %}
```json
{
    "result": {
        "amount": 10000.00000000,
        "confirmations": 1389,
        "blockhash": "24f64485ea05250ce75a73b3549afad05e078a7d8abd54e695a957dadee264fa",
        "blockindex": 0,
        "blocktime": 1632136777,
        "txid": "8e992e1512887da5e02fd52a1bd19d527ee54436934c215f0c23cf4ec04771e7",
        "time": 1632136777,
        "timereceived": 1632136777,
        "details": [
            {
                "account": "",
                "address": "DLGbK6mCjBT67r8wjJqCg8hkFiBYV5JquH",
                "category": "generate",
                "amount": 10000.00000000,
                "vout": 0
            }
        ],
        "hex": "01000000010000000000000000..."
    },
    "error": null,
    "id": "getblock.io"
}
```
{% endcode %}

## Response Parameters

| Field         | Type   | Description                        |
| ------------- | ------ | ---------------------------------- |
| amount        | number | The transaction amount in DOGE.    |
| confirmations | number | The number of confirmations.       |
| blockhash     | string | The block hash.                    |
| blockindex    | number | The index of transaction in block. |
| blocktime     | number | The time of the block.             |
| txid          | string | The transaction ID.                |
| time          | number | The transaction time.              |
| timereceived  | number | The time received by this node.    |
| details       | array  | Array of transaction details.      |
| hex           | string | Raw transaction hex.               |

## Use Case

The `gettransaction` method is essential for:

* Transaction status checking
* Payment confirmation
* Wallet transaction history
* Transaction details lookup
* Block explorer functionality

## Error Handling

| Error Code | Message                              | Cause                            |
| ---------- | ------------------------------------ | -------------------------------- |
| -5         | Invalid or non-wallet transaction id | Transaction not found.           |
| 403        | Forbidden                            | Missing or invalid ACCESS-TOKEN. |
