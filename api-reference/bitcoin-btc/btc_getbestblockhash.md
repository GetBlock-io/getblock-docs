---
description: >-
  Example code for the getbestblockhash JSON RPC method. Сomplete guide on how
  to use getbestblockhash JSON RPC in GetBlock Web3 documentation.
---

# getbestblockhash - Bitcoin

This method returns the hash of the best (tip) block in the most-work, fully validated chain.

### Parameters

* none

### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getbestblockhash",
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
    "method": "getbestblockhash",
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
    "method": "getbestblockhash",
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
            "method": "getbestblockhash",
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
    "result": "000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428"
}
```

### Response Parameters

| Field  | Type   | Description                                          |
| ------ | ------ | ---------------------------------------------------- |
| result | string | The block hash hex string of the current best block. |

### Use Case

The `getbestblockhash` method is essential for:

* Monitoring blockchain tip for new blocks
* Synchronizing with the latest chain state
* Building block explorers and dashboards
* Implementing chain reorganization detection
* Creating real-time blockchain monitoring tools
* Supporting payment confirmation tracking

### Error Handling

| Status Code | Error Message | Cause                            |
| ----------- | ------------- | -------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS-TOKEN. |

### Integration Notes

The `getbestblockhash` method helps developers:

* Track the current chain tip
* Build real-time block notification systems
* Implement chain synchronization logic
* Create blockchain monitoring dashboards
* Support transaction confirmation tracking
