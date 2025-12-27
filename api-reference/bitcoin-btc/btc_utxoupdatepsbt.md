---
description: >-
  Example code for the utxoupdatepsbt JSON-RPC method. Complete guide on how to
  use utxoupdatepsbt JSON-RPC in GetBlock Web3 documentation.
---

# utxoupdatepsbt - Bitcoin

This method updates all segwit inputs and outputs in a PSBT with data from output descriptors, the UTXO set, or both.

### Parameters

| Parameter   | Type   | Required | Description                                       |
| ----------- | ------ | -------- | ------------------------------------------------- |
| psbt        | string | Yes      | A base64 string of a PSBT.                        |
| descriptors | array  | No       | An array of output descriptor strings or objects. |

### Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "utxoupdatepsbt",
    "params": ["cHNidP8BAHUCAAAAASaBcTce3/KF6Tig7cez..."],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code overflow="wrap" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "utxoupdatepsbt",
    "params": ["cHNidP8BAHUCAAAAASaBcTce3/KF6Tig7cez..."],
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

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "utxoupdatepsbt",
    "params": ["cHNidP8BAHUCAAAAASaBcTce3/KF6Tig7cez..."],
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
{% code overflow="wrap" %}
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
            "method": "utxoupdatepsbt",
    "params": ["cHNidP8BAHUCAAAAASaBcTce3/KF6Tig7cez..."],
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
    "result": "cHNidP8BAHUCAAAAASaBcTce3/KF6Tig7cez..."
}
```

### Response Parameters

| Field  | Type   | Description                                                 |
| ------ | ------ | ----------------------------------------------------------- |
| result | string | The updated PSBT in base64 format with UTXO data filled in. |

### Use Case

The `utxoupdatepsbt` method is essential for:

* Adding UTXO data to PSBTs for signing
* Updating PSBTs with blockchain state
* Preparing PSBTs for hardware wallet signing
* Filling in missing witness UTXO information
* Supporting offline signing workflows
* Building complete PSBT signing pipelines

### Error Handling

| Status Code | Error Message      | Cause                                    |
| ----------- | ------------------ | ---------------------------------------- |
| 403         | Forbidden          | Missing or invalid ACCESS-TOKEN.         |
| -25         | Error parsing PSBT | The PSBT string is malformed or invalid. |

### Integration With Web3

The `utxoupdatepsbt` method helps developers:

* Prepare PSBTs for offline signing
* Fill in missing UTXO data from blockchain
* Support hardware wallet integrations
* Build complete signing workflows
* Enable watch-only wallet spending
