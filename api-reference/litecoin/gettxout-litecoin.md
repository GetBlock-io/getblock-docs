---
description: >-
  Example code for the gettxout JSON-RPC method. Complete guide on how to use
  gettxout JSON-RPC in GetBlock Web3 documentation.
---

# gettxout - Litecoin

Returns details about an unspent transaction output.

### Parameters

| Parameter        | Type    | Description                                             |
| ---------------- | ------- | ------------------------------------------------------- |
| txid             | string  | The transaction id.                                     |
| n                | number  | Vout number.                                            |
| include\_mempool | boolean | Optional. Whether to include the mempool. Default=true. |

### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "gettxout",
    "params": ["txid123...", 0, true],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
{% code title="axios.js" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "gettxout",
    "params": ["txid123...", 0, true],
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

{% tab title="Python" %}
{% code title="request.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "gettxout",
    "params": ["txid123...", 0, true],
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
            "method": "gettxout",
            "params": ["txid123...", 0, true],
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

### Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "bestblock": "blockhash...",
        "confirmations": 100,
        "value": 1.5,
        "scriptPubKey": {
            "asm": "OP_DUP OP_HASH160...",
            "hex": "76a914...",
            "type": "pubkeyhash",
            "address": "LTC1address..."
        },
        "coinbase": false
    }
}
```

### Response Parameters

| Field         | Type    | Description                          |
| ------------- | ------- | ------------------------------------ |
| confirmations | number  | Number of confirmations.             |
| value         | number  | The transaction value in LTC.        |
| scriptPubKey  | object  | The script key information.          |
| coinbase      | boolean | Whether it's a coinbase transaction. |

### Use Case

The `gettxout` method is essential for:

* UTXO verification
* Balance checking
* Transaction building
* Wallet development

### Error Handling

| Status Code | Error Message | Cause                            |
| ----------- | ------------- | -------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS-TOKEN. |

### Integration Notes

The `gettxout` method helps developers build robust applications that interact with the Litecoin blockchain.
