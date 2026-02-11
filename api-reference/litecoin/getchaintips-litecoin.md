---
description: >-
  Example code for the getchaintips JSON-RPC method. Complete guide on how to
  use getblock JSON-RPC in getchaintips Web3 documentation.
---

# getchaintips - Litecoin

Return information about all known tips in the block tree.

### Parameters

* None

### Request examples

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getchaintips",
    "params": [],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios (JavaScript)" %}
{% code title="axios.js" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "getchaintips",
    "params": [],
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

{% tab title="Python (requests)" %}
{% code title="request.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "getchaintips",
    "params": [],
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

{% tab title="Rust (reqwest)" %}
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
            "method": "getchaintips",
            "params": [],
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

{% code title="response.json" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": [{
        "height": 2750000,
        "hash": "blockhash...",
        "branchlen": 0,
        "status": "active"
    }]
}
```
{% endcode %}

### Response parameters

| Field     | Type   | Description                                                                     |
| --------- | ------ | ------------------------------------------------------------------------------- |
| height    | number | Height of the chain tip.                                                        |
| hash      | string | Block hash of the tip.                                                          |
| branchlen | number | Length of branch connecting the tip to main chain.                              |
| status    | string | Status of the chain (active, valid-fork, valid-headers, headers-only, invalid). |

### Use case

The getchaintips method is essential for:

* Chain reorganization detection
* Fork monitoring
* Chain analysis
* Network health monitoring

### Error handling

| Status Code | Error Message | Cause                            |
| ----------- | ------------- | -------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS-TOKEN. |

### Integration notes

The `getchaintips` method helps developers build robust applications that interact with the Litecoin blockchain.
