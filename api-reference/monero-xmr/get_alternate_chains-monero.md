---
description: >-
  Example code for the get_alternate_chains JSON-RPC method. Complete guide on
  how to use get_alternate_chains JSON-RPC in GetBlock Web3 documentation.
---

# get\_alternate\_chains - Monero

This method returns information about all known alternative chain tips — branches that diverged from the main chain but have not been pruned. Useful for monitoring chain health and reorg activity.

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
    "method": "get_alternate_chains",
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
    "method": "get_alternate_chains",
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
    "method": "get_alternate_chains",
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
        "method": "get_alternate_chains",
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
        "chains": [
            {
                "block_hash": "e22cf75f39ae720e8b71b3d120a5ac03f0db50bba6379e2850975b4859190bc6",
                "block_hashes": [
                    "e22cf75f39ae720e8b71b3d120a5ac03f0db50bba6379e2850975b4859190bc6"
                ],
                "difficulty": 100000000,
                "difficulty_top64": 0,
                "height": 1546000,
                "length": 1,
                "main_chain_parent_block": "37155cae7d3aa8ca7dc8b9e69e0d1f64b5e3a8b1c2d9e4f5a6b7c8d9e0f1a2b3",
                "wide_difficulty": "0x5f5e100"
            }
        ],
        "status": "OK",
        "untrusted": false
    }
}
```
{% endcode %}

## Response Parameters

| Field                                       | Type         | Description                                             |
| ------------------------------------------- | ------------ | ------------------------------------------------------- |
| result.chains                               | array        | List of alternative chain branches                      |
| result.chains\[].block\_hash                | string       | Hash of the tip block on this branch                    |
| result.chains\[].height                     | unsigned int | Height of the tip block on this branch                  |
| result.chains\[].length                     | unsigned int | Number of blocks in this branch                         |
| result.chains\[].difficulty                 | unsigned int | Cumulative difficulty of this branch                    |
| result.chains\[].main\_chain\_parent\_block | string       | Hash of the main-chain block where this branch diverged |

## Use Cases

* Reorg monitoring and analytics
* Network health dashboards
* Research into chain stability and finality

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
const result = await daemon.getDaemonConnection().sendJsonRequest('get_alternate_chains', {});
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
result = backend.raw_request('get_alternate_chains', {})
print(result)
```
{% endtab %}
{% endtabs %}
