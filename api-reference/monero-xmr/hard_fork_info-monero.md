---
description: >-
  Example code for the hard_fork_info JSON-RPC method. Complete guide on how to
  use hard_fork_info JSON-RPC in GetBlock Web3 documentation.
---

# hard\_fork\_info - Monero

This method returns information about the current and upcoming hard forks of the Monero protocol. Useful for clients that need to adapt behavior across protocol versions.

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "hard_fork_info",
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
    "method": "hard_fork_info",
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
    "method": "hard_fork_info",
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
        "method": "hard_fork_info",
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

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "credits": 0,
        "earliest_height": 2689608,
        "enabled": true,
        "state": 0,
        "status": "OK",
        "threshold": 0,
        "top_hash": "",
        "untrusted": false,
        "version": 16,
        "votes": 10080,
        "voting": 16,
        "window": 10080
    }
}
```

## Response Parameters

| Field                   | Type         | Description                                                                |
| ----------------------- | ------------ | -------------------------------------------------------------------------- |
| result.version          | unsigned int | Current protocol version                                                   |
| result.enabled          | boolean      | Whether the latest fork is enabled at the current height                   |
| result.state            | unsigned int | Fork state: `0` = updated/ready, `1` = behind, `2` = updating, `3` = ready |
| result.earliest\_height | unsigned int | Earliest height at which the next fork can activate                        |
| result.votes            | unsigned int | Number of votes in favor of the next version in the recent window          |
| result.threshold        | unsigned int | Number of votes required to activate                                       |
| result.window           | unsigned int | Size of the voting window in blocks                                        |

## Use Cases

* Detecting upcoming hard forks ahead of activation
* Client compatibility checks
* Monitoring validator/node update pace

## Error Handling

| Status Code | Error Message     | Cause                                       |
| ----------- | ----------------- | ------------------------------------------- |
| 404         | Not Found         | Missing or invalid `<ACCESS-TOKEN>`         |
| -32602      | Invalid params    | Request parameters are missing or malformed |
| 429         | Too Many Requests | Rate limit exceeded for your plan           |

## SDK Integration

{% tabs %}
{% tab title="monero-javascript" %}
```javascript
import monerojs from 'monero-javascript';

const daemon = await monerojs.connectToDaemonRpc('https://go.getblock.io/<ACCESS-TOKEN>/');

// Most methods are exposed directly on the daemon client. For raw access:
const result = await daemon.getDaemonConnection().sendJsonRequest('hard_fork_info', {});
console.log(result);
```
{% endtab %}

{% tab title="monero-python" %}
```python
from monero.backends.jsonrpc import JSONRPCDaemon
from monero.daemon import Daemon

backend = JSONRPCDaemon(host='go.getblock.io', port=443, path='/<ACCESS-TOKEN>/', protocol='https')
daemon = Daemon(backend)

# For methods covered by the typed API, use the daemon attribute directly.
# For raw RPC access:
result = backend.raw_request('hard_fork_info', {})
print(result)
```
{% endtab %}
{% endtabs %}
