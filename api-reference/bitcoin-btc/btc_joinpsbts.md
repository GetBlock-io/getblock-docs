---
description: >-
  Example code for the joinpsbts JSON-RPC method. Complete guide on how to use
  joinpsbts JSON-RPC in GetBlock Web3 documentation.
---

# joinpsbts - Bitcoin

This method combines multiple distinct PSBTs with distinct inputs and outputs into a single PSBT that incorporates inputs and outputs from all the PSBTs. No input in any of the PSBTs can be in more than one of the PSBTs.

### Parameters

| Parameter | Type  | Required | Description                                  |
| --------- | ----- | -------- | -------------------------------------------- |
| txs       | array | Yes      | An array of base64 strings of PSBTs to join. |

### Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "joinpsbts",
    "params": [["cHNidP8BAHUCAAAAASaBcTce...", "cHNidP8BAHUCAAAAATaBdTdf..."]],
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
    "method": "joinpsbts",
    "params": [["cHNidP8BAHUCAAAAASaBcTce...", "cHNidP8BAHUCAAAAATaBdTdf..."]],
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
    "method": "joinpsbts",
    "params": [["cHNidP8BAHUCAAAAASaBcTce...", "cHNidP8BAHUCAAAAATaBdTdf..."]],
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
           "method": "joinpsbts",
          "params": [["cHNidP8BAHUCAAAAASaBcTce...", "cHNidP8BAHUCAAAAATaBdTdf..."]],
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
    "result": "cHNidP8BAHsCAAAAAgaBcTce3/KF6Tig7cez..."
}
```

### Response Parameters

| Field  | Type   | Description                                                       |
| ------ | ------ | ----------------------------------------------------------------- |
| result | string | The base64-encoded PSBT containing all joined inputs and outputs. |

### Use Case

The `joinpsbts` method is essential for:

* CoinJoin transaction construction
* Payment batching from multiple sources
* Multi-party transaction building
* Collaborative spending protocols
* Privacy-enhancing transaction techniques
* Complex payment channel operations

### Error Handling

| Status Code | Error Message      | Cause                                        |
| ----------- | ------------------ | -------------------------------------------- |
| 403         | Forbidden          | Missing or invalid ACCESS-TOKEN.             |
| -8          | Inputs overlap     | One or more inputs appear in multiple PSBTs. |
| -25         | Error parsing PSBT | One or more PSBT strings are invalid.        |

### Integration With Web3

The `joinpsbts` method helps developers:

* Build CoinJoin implementations
* Create multi-party payment systems
* Implement payment batching
* Support collaborative transaction protocols
* Build privacy-focused applications
