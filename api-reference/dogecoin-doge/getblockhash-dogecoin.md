---
description: >-
  Example code for the getblockhash JSON-RPC method. Complete guide on how to
  use getblockhash JSON-RPC in GetBlock Web3 documentation.
---

# getblockhash - Dogecoin

This method returns the hash of a block at a given height in the best-block-chain.

## Parameters

| Parameter | Type   | Required | Description               |
| --------- | ------ | -------- | ------------------------- |
| height    | number | Yes      | The block height (index). |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code title="curl request" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getblockhash",
    "params": [3904788],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="axios example" %}
```javascript
const axios = require('axios');

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "getblockhash",
    "params": [3904788],
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

{% tab title="Request" %}
{% code title="python example" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "getblockhash",
    "params": [3904788],
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
{% code title="rust example" %}
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
            "method": "getblockhash",
            "params": [3904788],
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

## Response

{% code title="response.json" %}
```json
{
    "result": "24f64485ea05250ce75a73b3549afad05e078a7d8abd54e695a957dadee264fa",
    "error": null,
    "id": "getblock.io"
}
```
{% endcode %}

## Response Parameters

| Field  | Type   | Description                             |
| ------ | ------ | --------------------------------------- |
| result | string | The block hash at the specified height. |
| error  | null   | Error object (null if no error).        |
| id     | string | Request identifier.                     |

## Use Case

The `getblockhash` method is essential for:

* Converting block heights to hashes
* Block explorer navigation
* Historical data retrieval
* Chain traversal operations
* Synchronization checkpoints

## Error Handling

| Error Code | Message                   | Cause                               |
| ---------- | ------------------------- | ----------------------------------- |
| -8         | Block height out of range | Height exceeds current block count. |
| 403        | Forbidden                 | Missing or invalid ACCESS-TOKEN.    |
