# get\_net\_stats monero

This non-JSON-RPC endpoint returns cumulative network statistics for the node since it started — total bytes sent and received, total packets, etc.

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/get_net_stats' \
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
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/get_net_stats',
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

url = "https://go.getblock.io/<ACCESS-TOKEN>/get_net_stats"

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
        .post("https://go.getblock.io/<ACCESS-TOKEN>/get_net_stats")
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
    "start_time": 1614267442,
    "total_bytes_in": 1234567890,
    "total_bytes_out": 987654321,
    "total_packets_in": 12345,
    "total_packets_out": 9876,
    "status": "OK",
    "untrusted": false
}
```
{% endcode %}

## Response Parameters

| Field               | Type         | Description                                       |
| ------------------- | ------------ | ------------------------------------------------- |
| start\_time         | unsigned int | Unix timestamp at which the node started counting |
| total\_bytes\_in    | unsigned int | Total bytes received since `start_time`           |
| total\_bytes\_out   | unsigned int | Total bytes sent since `start_time`               |
| total\_packets\_in  | unsigned int | Total packets received                            |
| total\_packets\_out | unsigned int | Total packets sent                                |

## Use Cases

* Bandwidth-usage dashboards
* Capacity planning on dedicated nodes
* Detecting unusual P2P traffic patterns

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

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/get_net_stats', {});
console.log(response.data);
```
{% endtab %}

{% tab title="monero-python" %}
```python
# Non-JSON-RPC HTTP endpoints are not exposed by typed SDK methods.
# Use a plain HTTP client as shown in the Request Example above.
import requests

response = requests.post('https://go.getblock.io/<ACCESS-TOKEN>/get_net_stats', json={})
print(response.json())
```
{% endtab %}
{% endtabs %}
