---
description: >-
  Example code for the platform.getHeight JSON-RPC method. Complete guide on how
  to use platform.getHeight JSON-RPC in GetBlock Web3 documentation.
---

# platform.getHeight - AVAX

Returns the current P-Chain block height. P-Chain produces blocks via Snowman consensus and finalizes within sub-second timeframes.

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/P' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "platform.getHeight",
    "params": {},
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "platform.getHeight",
    "params": {},
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/P',
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

url = "https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/P"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "platform.getHeight",
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
        "method": "platform.getHeight",
        "params": {},
        "id": "getblock.io"
    });

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/P")
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
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "height": "5193707"
    }
}
```
{% endcode %}

## Response Parameters

| Field         | Type   | Description                                   |
| ------------- | ------ | --------------------------------------------- |
| result.height | string | Current P-Chain block height (decimal string) |

## Use Cases

* Polling P-Chain chain tip in indexers
* Health-checking P-Chain RPC connectivity
* Cross-referencing P-Chain heights with C-Chain in dashboards

## Error Handling

| Status Code | Error Message     | Cause                                       |
| ----------- | ----------------- | ------------------------------------------- |
| 404         | Not Found         | Missing or invalid `<ACCESS-TOKEN>`         |
| -32602      | Invalid params    | Request parameters are missing or malformed |
| 429         | Too Many Requests | Rate limit exceeded for your plan           |

## SDK Integration

{% tabs %}
{% tab title="AvalancheJS / Axios" %}
```javascript
// AvalancheJS provides typed clients for P-Chain and X-Chain.
// P-Chain operations are exposed through `pvm` in AvalancheJS.
// For raw JSON-RPC access, use a generic HTTP client as in the Request Example.
import axios from 'axios';

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/P', {
    jsonrpc: '2.0',
    id: 'getblock.io',
    method: 'platform.getHeight',
    params: {}
});
console.log(response.data);
```
{% endtab %}

{% tab title="Python" %}
```python
# AvalancheJS is JavaScript-only. For Python, use a generic JSON-RPC client.
import requests
import json

url = 'https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/P'
payload = {
    'jsonrpc': '2.0',
    'id': 'getblock.io',
    'method': 'platform.getHeight',
    'params': {}
}
response = requests.post(url, json=payload)
print(response.json())
```
{% endtab %}
{% endtabs %}
