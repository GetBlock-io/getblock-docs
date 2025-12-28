---
description: >-
  Example code for the getindexinfo JSON-RPC method. Complete guide on how to
  use getindexinfo JSON-RPC in GetBlock Web3 documentation.
---

# getindexinfo - Bitcoin

This method returns the status of one or more indices currently running on the node.

### Parameters

| Parameter   | Type   | Required | Description                                                                               |
| ----------- | ------ | -------- | ----------------------------------------------------------------------------------------- |
| index\_name | string | No       | Filter for a specific index name (e.g., "txindex", "coinstatsindex", "blockfilterindex"). |

### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getindexinfo",
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
    "method": "getindexinfo",
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
    "method": "getindexinfo",
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
            "method": "getindexinfo",
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
    "result": {
        "txindex": {
            "synced": true,
            "best_block_height": 820000
        },
        "blockfilterindex": {
            "synced": true,
            "best_block_height": 820000
        },
        "coinstatsindex": {
            "synced": true,
            "best_block_height": 820000
        }
    }
}
```

### Response Parameters

| Field               | Type    | Description                                       |
| ------------------- | ------- | ------------------------------------------------- |
| \[index\_name]      | object  | Object containing index status.                   |
| synced              | boolean | Whether the index is fully synced.                |
| best\_block\_height | number  | The block height up to which the index is synced. |

### Use Case

The `getindexinfo` method is essential for:

* Checking node index availability
* Verifying index sync status
* Debugging lookup failures
* Monitoring node health
* Validating feature availability
* Building admin dashboards

### Error Handling

| Status Code | Error Message      | Cause                                  |
| ----------- | ------------------ | -------------------------------------- |
| 403         | Forbidden          | Missing or invalid ACCESS-TOKEN.       |
| -8          | Unknown index name | The specified index name is not valid. |

### Integration With Web3

The `getindexinfo` method helps developers:

* Verify index availability before queries
* Build node monitoring tools
* Check sync progress for indices
* Ensure transaction lookups will work
* Debug index-related errors
