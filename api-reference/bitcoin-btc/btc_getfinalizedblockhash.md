---
description: >-
  Example code for the getfinalizedblockhash JSON-RPC method. Complete guide on
  how to use getfinalizedblockhash JSON-RPC in GetBlock Web3 documentation.
---

# getfinalizedblockhash - Bitcoin

This method returns the hash of the finalized block. A finalized block is a block that has been deeply confirmed and is considered irreversible.

### Parameters

* None

### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getfinalizedblockhash",
    "params": [],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "getfinalizedblockhash",
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
{% endtab %}

{% tab title="Request" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "getfinalizedblockhash",
    "params": [],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
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
            "method": "getfinalizedblockhash",
            "params": [],
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

### Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "000000000000000000043a9302e08c16ea186950f42a5498320ddd1bd7ab3420"
}
```

### Response Parameters

| Field  | Type   | Description                                      |
| ------ | ------ | ------------------------------------------------ |
| result | string | The hash of the finalized block as a hex string. |

### Use Case

The `getfinalizedblockhash` method is essential for:

* Determining transaction finality
* Building confirmation tracking systems
* Implementing safe payment thresholds
* Exchange deposit confirmation logic
* High-value transaction verification
* Building merchant payment systems

### Error Handling

| Status Code | Error Message      | Cause                            |
| ----------- | ------------------ | -------------------------------- |
| 403         | Forbidden          | Missing or invalid ACCESS-TOKEN. |
| -1          | No finalized block | No block has been finalized yet. |

### Integration With Web3

The `getfinalizedblockhash` method helps developers:

* Build payment confirmation systems
* Implement finality guarantees
* Create safe deposit tracking
* Support high-value transaction workflows
* Build exchange integration systems
