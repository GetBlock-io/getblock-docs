---
description: >-
  Example code for the getblockcount JSON-RPC method. Complete guide on how to
  use getblockcount JSON-RPC in GetBlock Web3 documentation.
---

# getblockcount - Bitcoin

This method returns the height of the most-work, fully validated chain. The genesis block has height 0.

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
    "method": "getblockcount",
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
    "method": "getblockcount",
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
    "method": "getblockcount",
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
            "method": "getblockcount",
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
    "result": 820000
}
```

### Response Parameters

| Field  | Type   | Description                                               |
| ------ | ------ | --------------------------------------------------------- |
| result | number | The current block height (number of blocks in the chain). |

### Use Case

The `getblockcount` method is essential for:

* Monitoring blockchain height in real-time
* Calculating transaction confirmations
* Tracking halving events and milestones
* Building blockchain dashboards
* Synchronization status monitoring
* Estimating time to future blocks

### Error Handling

| Status Code | Error Message | Cause                            |
| ----------- | ------------- | -------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS-TOKEN. |

### Integration Notes

The `getblockcount` method helps developers:

* Display current block height in UIs
* Calculate confirmation counts
* Track halving countdown
* Monitor chain progress
* Build height-based triggers
