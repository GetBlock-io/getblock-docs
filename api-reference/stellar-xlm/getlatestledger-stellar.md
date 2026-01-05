---
description: >-
  Example code for the getLatestLedger JSON-RPC method. Complete guide on how to
  use getLatestLedger JSON-RPC in GetBlock Web3 documentation.
---

# getLatestLedger - Stellar

This method returns information about the latest known ledger on the Stellar network.

### Parameters

* None

### Request

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getLatestLedger",
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "getLatestLedger",
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

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "getLatestLedger",
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
{% code title="example.rs" %}
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
            "method": "getLatestLedger",
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
    "result": {
        "id": "c73c5eac58a441d4eb733c35253ae85f783e018f7be5ef974258fed067aabb36",
        "protocolVersion": 20,
        "sequence": 2539605
    }
}
```

### Response Parameters

| Field           | Type    | Description                          |
| --------------- | ------- | ------------------------------------ |
| id              | string  | Hash identifier of the latest ledger |
| protocolVersion | integer | Current protocol version             |
| sequence        | integer | Sequence number of the latest ledger |

### Use Case

The `getLatestLedger` method is essential for:

* Monitoring network synchronization status
* Transaction confirmation tracking
* Building block explorers
* Determining current chain state
* Sync progress verification
* Protocol version checking

### Error Handling

| Status Code | Error Message       | Cause                           |
| ----------- | ------------------- | ------------------------------- |
| 403         | Forbidden           | Missing or invalid ACCESS-TOKEN |
| 503         | Service Unavailable | Node is not synced              |
