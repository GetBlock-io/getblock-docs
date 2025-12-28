---
description: >-
  Example code for the getconnectioncount JSON-RPC method. Complete guide on how
  to use getconnectioncount JSON-RPC in GetBlock Web3 documentation.
---

# getconnectioncount - Dogecoin

This method returns the number of connections to other nodes.

## Parameters

* None

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getconnectioncount",
    "params": [],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
const axios = require('axios');

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "getconnectioncount",
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
    "method": "getconnectioncount",
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
            "method": "getconnectioncount",
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

## Response

```json
{
    "result": 8,
    "error": null,
    "id": "getblock.io"
}
```

## Response Parameters

| Field  | Type   | Description                               |
| ------ | ------ | ----------------------------------------- |
| result | number | The number of connections to other nodes. |
| error  | null   | Error object (null if no error).          |
| id     | string | Request identifier.                       |

## Use Case

The `getconnectioncount` method is essential for:

* Node health monitoring
* Network connectivity verification
* Infrastructure monitoring
* Debugging connection issues
* P2P network analysis

## Error Handling

| Error Code | Message   | Cause                            |
| ---------- | --------- | -------------------------------- |
| 403        | Forbidden | Missing or invalid ACCESS-TOKEN. |
