---
description: >-
  Example code for the get_bans JSON-RPC method. Complete guide on how to use
  get_bans JSON-RPC in GetBlock Web3 documentation.
---

# get\_bans - Monero

This method returns the list of currently banned IP addresses on the node, along with the remaining time for each ban.

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "get_bans",
    "params": {},
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "get_bans",
    "params": {},
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
    .then(response => console.log(JSON.stringify(response.data, null, 2)))
    .catch(error => console.log(error));
```
{% endtab %}

{% tab title="Python (Requests)" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "get_bans",
    "params": {},
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endtab %}

{% tab title="Rust (Reqwest)" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let payload = json!({
        "jsonrpc": "2.0",
        "method": "get_bans",
        "params": {},
        "id": "getblock.io"
});

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&payload)
        .send()
        .await?;

    let result: serde_json::Value = response.json().await?;
    println!("{:#?}", result);

    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "bans": [
            {
                "host": "192.0.2.1",
                "ip": 16777343,
                "seconds": 3600
            }
        ],
        "status": "OK"
    }
}
```
{% endcode %}

## Response Parameters

| Field                  | Type         | Description                  |
| ---------------------- | ------------ | ---------------------------- |
| result.bans            | array        | List of active bans          |
| result.bans\[].host    | string       | Banned host (IP or hostname) |
| result.bans\[].seconds | unsigned int | Seconds remaining on the ban |

## Use Cases

* Reviewing ban state on a dedicated node
* Detecting persistent bad-actor IPs

## Error Handling

| Status Code | Error Message     | Cause                                                |
| ----------- | ----------------- | ---------------------------------------------------- |
| 404         | Not Found         | Missing or invalid `<ACCESS-TOKEN>`                  |
| -32601      | Method not found  | Method does not exist or is not enabled on this node |
| 429         | Too Many Requests | Rate limit exceeded for your plan                    |
