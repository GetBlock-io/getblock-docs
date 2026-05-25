# get\_peer\_list monero

This non-JSON-RPC endpoint returns the node's white and gray peer lists — white peers are known-good, gray peers are recently seen but unverified.

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/get_peer_list' \
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
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/get_peer_list',
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

url = "https://go.getblock.io/<ACCESS-TOKEN>/get_peer_list"

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
        .post("https://go.getblock.io/<ACCESS-TOKEN>/get_peer_list")
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
    "gray_list": [
        {
            "host": "192.0.2.2",
            "id": 1234567890,
            "ip": 33554562,
            "last_seen": 1737869732,
            "port": 18080,
            "rpc_credits_per_hash": 0,
            "rpc_port": 0
        }
    ],
    "white_list": [
        {
            "host": "192.0.2.1",
            "id": 9876543210,
            "ip": 16777343,
            "last_seen": 1737869732,
            "port": 18080,
            "rpc_credits_per_hash": 0,
            "rpc_port": 0
        }
    ],
    "status": "OK",
    "untrusted": false
}
```
{% endcode %}

## Response Parameters

| Field                     | Type         | Description                        |
| ------------------------- | ------------ | ---------------------------------- |
| white\_list               | array        | Known-good peers                   |
| gray\_list                | array        | Recently seen but unverified peers |
| white\_list\[].host       | string       | Peer host                          |
| white\_list\[].port       | unsigned int | Peer port                          |
| white\_list\[].last\_seen | unsigned int | Unix timestamp of last contact     |

## Use Cases

* Network topology research
* Peer-set health analytics
* Detecting suspicious patterns in known peers

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

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/get_peer_list', {});
console.log(response.data);
```
{% endtab %}

{% tab title="monero-python" %}
```python
// Non-JSON-RPC HTTP endpoints are not exposed by typed SDK methods.
// Use a plain HTTP client as shown in the Request Example above.
import requests

response = requests.post('https://go.getblock.io/<ACCESS-TOKEN>/get_peer_list', json={})
print(response.json())
```
{% endtab %}
{% endtabs %}
