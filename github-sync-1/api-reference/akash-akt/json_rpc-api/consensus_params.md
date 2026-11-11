---
description: >-
  Example code for the consensus_params JSON-RPC method. Complete guide on how
  to use consensus_params JSON-RPC in GetBlock Web3 documentation.
---

# consensus\_params - Akash

Returns the consensus parameters (block, evidence, validator) at a height.

{% hint style="warning" %}
**Shared Akash nodes are pruned.** Heights below roughly 26,980,292 are not retained and return
`-32603 Internal error` with a message naming the lowest available height. Read that floor from
[status](status.md) under `sync_info.earliest_block_height` before requesting historical data.
{% endhint %}

## Parameters

| Parameter | Type   | Required | Description                   |
| --------- | ------ | -------- | ----------------------------- |
| height    | string | Optional | Block height; omit for latest |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "consensus_params",
    "params": {"height": "28742282"}
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'consensus_params', params: {"height": "28742282"} }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'consensus_params', 'params': {"height": "19500000"}})
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="example.rs" %}
```rust
use reqwest::Client;
use serde_json::{json, Value};

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"consensus_params","params":{"height": "28742282"}})).send().await?.json::<Value>().await?;
    println!("{}", res["result"]);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "block_height": "28742282",
        "consensus_params": {
            "block": {
                "max_bytes": "22020096",
                "max_gas": "-1"
            },
            "evidence": {
                "max_age_num_blocks": "279138",
                "max_age_duration": "1814400000000000",
                "max_bytes": "0"
            },
            "validator": {
                "pub_key_types": [
                    "ed25519"
                ]
            },
            "version": {
                "app": "0"
            },
            "abci": {
                "vote_extensions_enable_height": "0"
            }
        }
    }
}
```

## Response Fields

| Field             | Type   | Description                     |
| ----------------- | ------ | ------------------------------- |
| consensus\_params | object | Block/evidence/validator params |
| block\_height     | string | Height the params apply to      |

## Use Cases

* **Limits**: Read block size and gas limits
* **Governance**: Track parameter changes

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| -32603 / Internal error   | Internal error | Height out of range                               |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
