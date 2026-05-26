---
description: >-
  Example code for the set_bans JSON-RPC method. Complete guide on how to use
  set_bans JSON-RPC in GetBlock Web3 documentation.
---

# set\_bans - Monero

This method adds or removes bans on one or more IP addresses. Administrative operation.

## Parameters

| Parameter       | Type         | Required | Description                     |
| --------------- | ------------ | -------- | ------------------------------- |
| bans            | array        | Yes      | Array of ban objects to apply   |
| bans\[].host    | string       | Yes      | IP address or hostname to ban   |
| bans\[].ban     | boolean      | Yes      | `true` to ban, `false` to unban |
| bans\[].seconds | unsigned int | Yes      | Duration of the ban in seconds  |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "set_bans",
    "params": {
        "bans": [
            {
                "host": "192.0.2.1",
                "ban": true,
                "seconds": 30
            }
        ]
    },
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "set_bans",
    "params": {
        "bans": [
            {
                "host": "192.0.2.1",
                "ban": true,
                "seconds": 30
            }
        ]
    },
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
    "method": "set_bans",
    "params": {
        "bans": [
            {
                "host": "192.0.2.1",
                "ban": true,
                "seconds": 30
            }
        ]
    },
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
        "method": "set_bans",
        "params": {
                "bans": [
                        {
                                "host": "192.0.2.1",
                                "ban": true,
                                "seconds": 30
                        }
                ]
        },
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
        "status": "OK"
    }
}
```
{% endcode %}

## Response Parameters

| Field         | Type   | Description                   |
| ------------- | ------ | ----------------------------- |
| result.status | string | `OK` if the bans were applied |

## Use Cases

* Manually banning misbehaving peers on a dedicated node
* Implementing custom peer-management policies

## Error Handling

| Status Code | Error Message     | Cause                                       |
| ----------- | ----------------- | ------------------------------------------- |
| 404         | Not Found         | Missing or invalid `<ACCESS-TOKEN>`         |
| -32602      | Invalid params    | Request parameters are missing or malformed |
| 429         | Too Many Requests | Rate limit exceeded for your plan           |
