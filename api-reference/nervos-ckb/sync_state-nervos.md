---
description: >-
  Example code for the sync_state JSON-RPC method. Complete guide on how to use
  sync_state JSON-RPC in GetBlock Web3 documentation.
---

# sync\_state - Nervos

Returns the chain synchronization state of this node — total best known header, currently fast-syncing block, in-flight blocks, current low time.

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
    "method": "sync_state",
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
    "method": "sync_state",
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
    "method": "sync_state",
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
        "method": "sync_state",
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
    "result": {
        "best_known_block_number": "0xe5a45f",
        "best_known_block_timestamp": "0x18f3b8d2a3c",
        "fast_time": "0x1f4",
        "ibd": false,
        "inflight_blocks_count": "0x0",
        "low_time": "0x9c4",
        "normal_time": "0x3e8",
        "orphan_blocks_count": "0x0",
        "tip_hash": "0x02530b25ad0ff677acc365cb73de3e8cc09c7ddd58272e879252e199d08df83b",
        "tip_number": "0xe5a45f",
        "unverified_tip_hash": "0x02530b25ad0ff677acc365cb73de3e8cc09c7ddd58272e879252e199d08df83b",
        "unverified_tip_number": "0xe5a45f"
    }
}
```
{% endcode %}

## Response Parameters

| Field                             | Type    | Description                                          |
| --------------------------------- | ------- | ---------------------------------------------------- |
| result.ibd                        | boolean | `true` if the node is in Initial Block Download mode |
| result.best\_known\_block\_number | Uint64  | Highest block number known across all peers          |
| result.tip\_number                | Uint64  | Local tip block number                               |
| result.inflight\_blocks\_count    | Uint64  | Blocks currently being downloaded from peers         |
| result.orphan\_blocks\_count      | Uint64  | Orphan blocks in local storage                       |
| result.normal\_time               | Uint64  | Median time for block download (ms)                  |
| result.low\_time                  | Uint64  | p25 time for block download (ms)                     |
| result.fast\_time                 | Uint64  | p75 time for block download (ms)                     |

## Use Cases

* Detecting that a node is still in IBD before serving reads
* Monitoring sync progress in node dashboards
* Failover logic that prefers fully-synced nodes

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
const result = await rpc.send('sync_state', []);
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
    'method': 'sync_state',
    'params': []
}})
print(response.json())
```
{% endtab %}
{% endtabs %}
