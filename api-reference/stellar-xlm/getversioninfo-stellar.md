---
description: >-
  Example code for the getVersionInfo JSON-RPC method. Complete guide on how to
  use getVersionInfo JSON-RPC in GetBlock Web3 documentation.
---

# getVersionInfo - Stellar

This method returns version information about the Stellar RPC server.

### Parameters

* None

### Request

{% tabs %}
{% tab title="cURL" %}
{% code title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getVersionInfo",
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "getVersionInfo",
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
    "method": "getVersionInfo",
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
            "method": "getVersionInfo",
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

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "version": "25.0.0-5084a53192b63655c8c5ef1cdc793d2098cf785a",
        "commitHash": "5084a53192b63655c8c5ef1cdc793d2098cf785a",
        "buildTimestamp": "2025-12-12T21:41:24",
        "captiveCoreVersion": "stellar-core 25.0.0 (e9748b05a70d613437a52c8388dc0d8e68149394)",
        "protocolVersion": 24
    }
}
```
{% endcode %}

### Response Parameters

| Field              | Type    | Description                          |
| ------------------ | ------- | ------------------------------------ |
| version            | string  | Version of the Stellar RPC server    |
| commitHash         | string  | Git commit hash of the build         |
| buildTimestamp     | string  | Timestamp when the version was built |
| captiveCoreVersion | string  | Version of Captive Stellar Core      |
| protocolVersion    | integer | Current protocol version             |

### Use Case

The `getVersionInfo` method is essential for:

* Version compatibility checking
* Debugging node issues
* Infrastructure monitoring
* Feature availability detection
* Client compatibility verification
* Upgrade planning

### Error Handling

| Status Code | Error Message | Cause                           |
| ----------- | ------------- | ------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS-TOKEN |
