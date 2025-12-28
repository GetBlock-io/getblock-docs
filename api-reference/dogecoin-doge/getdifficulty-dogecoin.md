---
description: >-
  Example code for the getdifficulty JSON-RPC method. Complete guide on how to
  use getdifficulty JSON-RPC in GetBlock Web3 documentation.
---

# getdifficulty - Dogecoin

This method returns the current mining difficulty.

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
    "method": "getdifficulty",
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
    "method": "getdifficulty",
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
    "method": "getdifficulty",
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
            "method": "getdifficulty",
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

{% code title="Response JSON" %}
```json
{
    "result": 3525264.838757254,
    "error": null,
    "id": "getblock.io"
}
```
{% endcode %}

## Response Parameters

| Field  | Type   | Description                           |
| ------ | ------ | ------------------------------------- |
| result | number | The current proof-of-work difficulty. |
| error  | null   | Error object (null if no error).      |
| id     | string | Request identifier.                   |

## Use Case

The `getdifficulty` method is essential for:

* Mining pool difficulty monitoring
* Network hashrate estimation
* Mining profitability calculations
* Blockchain statistics dashboards
* Historical difficulty analysis

## Error Handling

| Error Code | Message   | Cause                            |
| ---------- | --------- | -------------------------------- |
| 403        | Forbidden | Missing or invalid ACCESS-TOKEN. |
