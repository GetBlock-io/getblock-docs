# get\_limit monero

This non-JSON-RPC endpoint returns the current peer-to-peer bandwidth limits configured on the node, in kB/s.

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/get_limit' \
--header 'Content-Type: application/json' \
--data-raw '{}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/get_limit',
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

url = "https://go.getblock.io/<ACCESS-TOKEN>/get_limit"

payload = json.dumps({})

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
    const client = Client::new();

    let payload = json!({});

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/get_limit")
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
    "limit_down": 8192,
    "limit_up": 2048,
    "status": "OK",
    "untrusted": false
}
```
{% endcode %}

## Response Parameters

| Field       | Type         | Description                      |
| ----------- | ------------ | -------------------------------- |
| limit\_down | unsigned int | Download bandwidth limit in kB/s |
| limit\_up   | unsigned int | Upload bandwidth limit in kB/s   |
| status      | string       | `OK` on success                  |

## Use Cases

* Inspecting node configuration
* Monitoring P2P bandwidth tier settings on dedicated nodes

## Error Handling

| Status Code | Error Message     | Cause                                                |
| ----------- | ----------------- | ---------------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                  |
| -32602      | Invalid params    | Request parameters are missing or malformed          |
| -32601      | Method not found  | Method does not exist or is not enabled on this node |
| 429         | Too Many Requests | Rate limit exceeded for your plan                    |

## SDK Integration

{% tabs %}
{% tab title="monero-javascript" %}
```javascript
// Non-JSON-RPC HTTP endpoints are not exposed by typed SDK methods.
// Use a plain HTTP client as shown in the Request Example above.
import axios from 'axios';

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/get_limit', {});
console.log(response.data);
```
{% endtab %}

{% tab title="monero-python" %}
```python
# Non-JSON-RPC HTTP endpoints are not exposed by typed SDK methods.
# Use a plain HTTP client as shown in the Request Example above.
import requests

response = requests.post('https://go.getblock.io/<ACCESS-TOKEN>/get_limit', json={})
print(response.json())
```
{% endtab %}
{% endtabs %}
