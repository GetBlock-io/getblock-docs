# get\_height monero

This non-JSON-RPC endpoint returns the current height of the node. It is the lightest possible call to determine the chain tip. **Note**: the canonical recommendation from Monero is to prefer the JSON-RPC `get_info` or `get_last_block_header` calls over this endpoint where possible.

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/get_height' \
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
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/get_height',
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

url = "https://go.getblock.io/<ACCESS-TOKEN>/get_height"

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
    let client = Client::new();

    let payload = json!({});

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/get_height")
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
    "hash": "e22cf75f39ae720e8b71b3d120a5ac03f0db50bba6379e2850975b4859190bc6",
    "height": 3194582,
    "status": "OK",
    "untrusted": false
}
```
{% endcode %}

## Response Parameters

| Field     | Type         | Description                          |
| --------- | ------------ | ------------------------------------ |
| height    | unsigned int | Current chain height                 |
| hash      | string       | Hash of the current top block        |
| status    | string       | `OK` on success                      |
| untrusted | boolean      | `true` if served from bootstrap mode |

## Use Cases

* Lightweight chain-tip polling where JSON-RPC overhead is unwanted
* Legacy clients that predate the JSON-RPC interface

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

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/get_height', {});
console.log(response.data);
```
{% endtab %}

{% tab title="monero-python" %}
```python
// Non-JSON-RPC HTTP endpoints are not exposed by typed SDK methods.
// Use a plain HTTP client as shown in the Request Example above.
import requests

response = requests.post('https://go.getblock.io/<ACCESS-TOKEN>/get_height', json={})
print(response.json())
```
{% endtab %}
{% endtabs %}
