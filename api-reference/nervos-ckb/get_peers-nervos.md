---
description: >-
  Example code for the get_peers JSON-RPC method. Complete guide on how to use
  get_peers JSON-RPC in GetBlock Web3 documentation.
---

# get\_peers - Nervos

Returns the connected peers — their node IDs, addresses, connection durations, and supported protocols.

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
    "method": "get_peers",
    "params": [],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "get_peers",
    "params": [],
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
    "method": "get_peers",
    "params": [],
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
        "method": "get_peers",
        "params": [],
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
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": [
        {
            "addresses": [
                {
                    "address": "/ip4/192.0.2.1/tcp/8115/p2p/QmUsZHPbjjzU627UZFt4k8j6ycEcNvXRnVGxCPKqwbAfQS",
                    "score": "0xff"
                }
            ],
            "connected_duration": "0x1c20",
            "is_outbound": true,
            "last_ping_duration": "0x32",
            "node_id": "QmUsZHPbjjzU627UZFt4k8j6ycEcNvXRnVGxCPKqwbAfQS",
            "protocols": [
                {
                    "id": "0x0",
                    "version": "0.0.1"
                }
            ],
            "sync_state": {
                "best_known_header_hash": "0x02530b25ad0ff677acc365cb73de3e8cc09c7ddd58272e879252e199d08df83b",
                "best_known_header_number": "0xe5a45f",
                "last_common_header_hash": "0x02530b25ad0ff677acc365cb73de3e8cc09c7ddd58272e879252e199d08df83b",
                "last_common_header_number": "0xe5a45f",
                "unknown_header_list_size": "0x0",
                "inflight_count": "0x0"
            },
            "version": "0.117.0"
        }
    ]
}
```
{% endcode %}

## Response Parameters

| Field                          | Type                | Description                                                  |
| ------------------------------ | ------------------- | ------------------------------------------------------------ |
| result                         | array of RemoteNode | Connected peers                                              |
| result\[].node\_id             | string              | Peer node ID                                                 |
| result\[].is\_outbound         | boolean             | Whether the connection was initiated outbound from this node |
| result\[].connected\_duration  | Uint64              | Connection duration in milliseconds                          |
| result\[].last\_ping\_duration | Uint64              | Last ping round-trip time in milliseconds                    |
| result\[].version              | string              | Peer's CKB version                                           |
| result\[].sync\_state          | PeerSyncState       | Per-peer sync state                                          |

## Use Cases

* Network topology inspection
* Detecting nodes with poor peer connectivity
* Investigating sync issues

## Error Handling

| Status Code | Error Message     | Cause                                                |
| ----------- | ----------------- | ---------------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                  |
| -32602      | Invalid params    | Request parameters are missing or malformed          |
| -32601      | Method not found  | Method does not exist or is not enabled on this node |
| -32603      | Internal error    | Server-side error while processing the request       |
| 429         | Too Many Requests | Rate limit exceeded for your plan                    |

## SDK Integration

{% tabs %}
{% tab title="@ckb-lumos/lumos" %}
```javascript
import { RPC } from '@ckb-lumos/rpc';

const rpc = new RPC('https://go.getblock.io/<ACCESS-TOKEN>/');

// Lumos provides typed methods on `rpc` (e.g. rpc.getTipHeader(), rpc.getBlock(hash)).
// For raw access to any CKB RPC method:
const result = await rpc.send('get_peers', []);
console.log(result);
```
{% endtab %}

{% tab title="ckb-sdk-python / requests" %}
```python
# Using the official CKB SDK Python (ckb-sdk-python).
# For methods not yet wrapped, fall back to raw requests.
import requests

response = requests.post('https://go.getblock.io/<ACCESS-TOKEN>/', json={
    'jsonrpc': '2.0',
    'id': 'getblock.io',
    'method': 'get_peers',
    'params': []
}})
print(response.json())
```
{% endtab %}
{% endtabs %}
