---
description: >-
  Example code for the createrawtransaction JSON-RPC method. Complete guide on
  how to use createrawtransaction JSON-RPC in GetBlock Web3 documentation.
---

# createrawtransaction - Dogecoin

This method creates a raw transaction spending given inputs.

{% hint style="info" %}
* This creates an unsigned transaction
* Use `signrawtransaction` to sign before broadcasting
* Ensure inputs have sufficient value for outputs plus fees
{% endhint %}

## Parameters

| Parameter | Type   | Required | Description                        |
| --------- | ------ | -------- | ---------------------------------- |
| inputs    | array  | Yes      | Array of transaction inputs.       |
| outputs   | object | Yes      | Object with addresses and amounts. |

### Input Object Structure

```json
{
    "txid": "transaction_id",
    "vout": 0
}
```

### Output Object Structure

```json
{
    "address": amount
}
```

## Request

{% tabs %}
{% tab title="cURL" %}
{% code title="curl" overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "createrawtransaction",
    "params": [
        [{"txid": "8e992e1512887da5e02fd52a1bd19d527ee54436934c215f0c23cf4ec04771e7", "vout": 0}],
        {"DLGbK6mCjBT67r8wjJqCg8hkFiBYV5JquH": 100.0}
    ],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="index.js" overflow="wrap" %}
```javascript
const axios = require('axios');

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "createrawtransaction",
    "params": [
        [{"txid": "8e992e1512887da5e02fd52a1bd19d527ee54436934c215f0c23cf4ec04771e7", "vout": 0}],
        {"DLGbK6mCjBT67r8wjJqCg8hkFiBYV5JquH": 100.0}
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
{% code title="example.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "createrawtransaction",
    "params": [
        [{"txid": "8e992e1512887da5e02fd52a1bd19d527ee54436934c215f0c23cf4ec04771e7", "vout": 0}],
        {"DLGbK6mCjBT67r8wjJqCg8hkFiBYV5JquH": 100.0}
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
            "method": "createrawtransaction",
            "params": [
                [{"txid": "8e992e1512887da5e02fd52a1bd19d527ee54436934c215f0c23cf4ec04771e7", "vout": 0}],
                {"DLGbK6mCjBT67r8wjJqCg8hkFiBYV5JquH": 100.0}
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

{% code overflow="wrap" %}
```json
{
    "result": "0100000001e77147c04ecf230c5f214c933644e57e529dd11b2ad52fe0a57d8812152e998e0000000000ffffffff0100e40b54020000001976a914a5f4d12ce3685781b227c1f39548ddef429e978388ac00000000",
    "error": null,
    "id": "getblock.io"
}
```
{% endcode %}

## Response Parameters

| Field  | Type   | Description                             |
| ------ | ------ | --------------------------------------- |
| result | string | Hex-encoded raw transaction (unsigned). |
| error  | null   | Error object (null if no error).        |
| id     | string | Request identifier.                     |

## Use Case

The `createrawtransaction` method is essential for:

* Building custom transactions
* Payment systems
* Batch transaction creation
* Multi-signature workflows
* Exchange systems

## Error Handling

| Error Code | Message           | Cause                             |
| ---------- | ----------------- | --------------------------------- |
| -8         | Invalid parameter | Invalid inputs or outputs format. |
| -5         | Invalid address   | Invalid Dogecoin address.         |
| 403        | Forbidden         | Missing or invalid ACCESS-TOKEN.  |
| -32700     | Parsed error      | Syntax error                      |
