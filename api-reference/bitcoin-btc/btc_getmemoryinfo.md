---
description: >-
  Example code for the getmemoryinfo JSON-RPC method. Complete guide on how to
  use getmemoryinfo JSON-RPC in GetBlock Web3 documentation.
---

# getmemoryinfo - Bitcoin

This method returns information about memory usage by the node.

### Parameters

| Parameter | Type   | Required | Description                                                                               |
| --------- | ------ | -------- | ----------------------------------------------------------------------------------------- |
| mode      | string | No       | "stats" for general statistics, "mallocinfo" for detailed malloc info (default: "stats"). |

### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getmemoryinfo",
    "params": ["stats"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "getmemoryinfo",
    "params": ["stats"],
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
    "method": "getmemoryinfo",
    "params": ["stats"],
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
            "method": "getmemoryinfo",
            "params": ["stats"],
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
        "locked": {
            "used": 32768,
            "free": 229376,
            "total": 262144,
            "locked": 262144,
            "chunks_used": 2,
            "chunks_free": 1
        }
    }
}
```

### Response Parameters

| Field               | Type   | Description                                  |
| ------------------- | ------ | -------------------------------------------- |
| locked              | object | Information about locked memory manager.     |
| locked.used         | number | Number of bytes used.                        |
| locked.free         | number | Number of bytes available in current arenas. |
| locked.total        | number | Total number of bytes managed.               |
| locked.locked       | number | Amount of bytes currently locked.            |
| locked.chunks\_used | number | Number of allocated chunks.                  |
| locked.chunks\_free | number | Number of unused chunks.                     |

### Use Case

The `getmemoryinfo` method is essential for:

* Monitoring node memory usage
* Diagnosing memory-related issues
* Performance optimization analysis
* Building node health dashboards
* Capacity planning
* Debugging memory leaks

### Error Handling

| Status Code | Error Message | Cause                            |
| ----------- | ------------- | -------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS-TOKEN. |
| -8          | Invalid mode  | The specified mode is not valid. |

### Integration With Web3

The `getmemoryinfo` method helps developers:

* Monitor node resource usage
* Build operational dashboards
* Diagnose performance issues
* Track memory allocation patterns
* Support node maintenance workflows
