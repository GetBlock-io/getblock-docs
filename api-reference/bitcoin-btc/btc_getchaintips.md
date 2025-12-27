---
description: >-
  Example code for the getchaintips JSON-RPC method. Complete guide on how to
  use getchaintips JSON-RPC in GetBlock Web3 documentation.
---

# getchaintips - Bitcoin

This method returns information about all known tips in the block tree, including the main chain and orphaned branches.

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
    "method": "getchaintips",
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
{% endtab %}

{% tab title="Request" %}
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

```python
message = "hello world"
print(message)
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
{% endtab %}
{% endtabs %}

### Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": [
        {
            "height": 820000,
            "hash": "000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428",
            "branchlen": 0,
            "status": "active"
        },
        {
            "height": 819998,
            "hash": "000000000000000000047c9402e08c16ea186950f42a5498320ddd1bd7ab3425",
            "branchlen": 1,
            "status": "valid-fork"
        },
        {
            "height": 819990,
            "hash": "000000000000000000048d9502e08c16ea186950f42a5498320ddd1bd7ab3420",
            "branchlen": 3,
            "status": "headers-only"
        }
    ]
}
```

### Response Parameters

| Field     | Type   | Description                                                                                 |
| --------- | ------ | ------------------------------------------------------------------------------------------- |
| height    | number | Height of the chain tip.                                                                    |
| hash      | string | Block hash of the tip.                                                                      |
| branchlen | number | Length of branch connecting to main chain (0 for main chain).                               |
| status    | string | Status of the chain: "active", "valid-fork", "valid-headers", "headers-only", or "invalid". |

| Status        | Description                                                         |
| ------------- | ------------------------------------------------------------------- |
| active        | This is the tip of the most-work chain.                             |
| valid-fork    | This branch is not part of the active chain but is fully validated. |
| valid-headers | Headers are valid but block data not available.                     |
| headers-only  | Only headers received, not validated.                               |
| invalid       | Block or one of its ancestors is invalid.                           |

### Use Case

The `getchaintips` method is essential for:

* Monitoring chain reorganizations
* Detecting potential 51% attacks
* Analyzing blockchain forks
* Building chain visualization tools
* Debugging node synchronization issues
* Monitoring network consensus

### Error Handling

| Status Code | Error Message       | Cause                            |
| ----------- | ------------------- | -------------------------------- |
| 403         | RBAC: access denied | Missing or invalid ACCESS-TOKEN. |

### Integration With Web3

The `getchaintips` method helps developers:

* Build chain reorganization monitors
* Create fork detection systems
* Implement consensus monitoring
* Visualize blockchain structure
* Analyze chain competition
