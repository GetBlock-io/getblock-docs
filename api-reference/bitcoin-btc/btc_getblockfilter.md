---
description: >-
  Example code for the getblockfilter JSON-RPC method. Complete guide on how to
  use getblockfilter JSON-RPC in GetBlock Web3 documentation.
---

# getblockfilter - Bitcoin {disallowed}

This method retrieves a BIP 157 content filter for a particular block.

### Parameters

| Parameter  | Type   | Required | Description                                     |
| ---------- | ------ | -------- | ----------------------------------------------- |
| blockhash  | string | Yes      | The hash of the block.                          |
| filtertype | string | No       | The type name of the filter (default: "basic"). |

### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getblockfilter",
    "params": ["000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428", "basic"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "getblockfilter",
    "params": ["000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428", "basic"],
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

{% tab title="request" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "getblockfilter",
    "params": ["000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428", "basic"],
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
            "method": "getblockfilter",
            "params": ["000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428", "basic"],
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
    "result": {
        "filter": "0193b2a0c1d2e3f4a5b6c7d8e9f0a1b2c3d4e5f6...",
        "header": "a1b2c3d4e5f6789abcdef0123456789abcdef01234567890abcdef1234567890"
    }
}
```

### Response Parameters

| Field  | Type   | Description                    |
| ------ | ------ | ------------------------------ |
| filter | string | The hex-encoded filter data.   |
| header | string | The hex-encoded filter header. |

### Use Case

The `getblockfilter` method is essential for:

* Light client implementations (BIP 157/158)
* Efficient address balance scanning
* Privacy-preserving blockchain queries
* Mobile wallet synchronization
* Reducing bandwidth for SPV clients
* Building neutrino-compatible applications

### Error Handling

| Status Code | Error Message     | Cause                                          |
| ----------- | ----------------- | ---------------------------------------------- |
| 403         | Forbidden         | Missing or invalid ACCESS-TOKEN.               |
| -5          | Block not found   | The specified block hash does not exist.       |
| -1          | Index not enabled | Block filter index is not enabled on the node. |

### Integration With Web3

The `getblockfilter` method helps developers:

* Build light wallet implementations
* Reduce bandwidth requirements for clients
* Implement BIP 157/158 protocols
* Create privacy-focused wallet scanners
* Support mobile and resource-constrained devices
