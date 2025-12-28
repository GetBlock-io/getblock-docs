---
description: >-
  Example code for the preciousblock JSON-RPC method. Complete guide on how to
  use preciousblock JSON-RPC in GetBlock Web3 documentation.
---

# preciousblock - Bitcoin

This method treats a block as if it were received before others with the same work. A later call can override the effect of an earlier one. The effects of this method are not retained across restarts.

### Parameters

| Parameter | Type   | Required | Description                                |
| --------- | ------ | -------- | ------------------------------------------ |
| blockhash | string | Yes      | The hash of the block to mark as precious. |

### Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "preciousblock",
    "params": ["000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428"],
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
    "method": "preciousblock",
    "params": ["000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428"],
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
   "method": "preciousblock",
    "params": ["000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428"],
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
           "method": "preciousblock",
    "params": ["000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428"],
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
    "result": null
}
```

### Response Parameters

| Field  | Type | Description              |
| ------ | ---- | ------------------------ |
| result | null | Returns null on success. |

### Use Case

The `preciousblock` method is essential for:

* Resolving chain tip conflicts
* Testing chain reorganization behavior
* Favoring specific chain branches
* Mining pool conflict resolution
* Advanced node management
* Research and testing scenarios

### Error Handling

| Status Code | Error Message   | Cause                                    |
| ----------- | --------------- | ---------------------------------------- |
| 403         | Forbidden       | Missing or invalid ACCESS-TOKEN.         |
| -5          | Block not found | The specified block hash does not exist. |

### Integration With Web3

The `preciousblock` method helps developers:

* Manage chain tip preferences
* Test reorganization scenarios
* Handle competing chain tips
* Support advanced node operations
* Research consensus behavior
