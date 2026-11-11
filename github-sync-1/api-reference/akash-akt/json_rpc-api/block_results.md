---
description: >-
  Example code for the block_results JSON-RPC method. Complete guide on how to
  use block_results JSON-RPC in GetBlock Web3 documentation.
---

# block\_results - Akash

Returns the ABCI results and events for every transaction in a block, plus block-level events. The primary source for indexing events.

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
    "method": "block_results",
    "params": {"height": "28742282"}
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'block_results', params: {"height": "28742282"} }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'block_results', 'params': {"height": "19500000"}})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"block_results","params":{"height": "28742282"}})).send().await?.json::<Value>().await?;
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
        "app_hash": "RmbhphlVUKC7xb6qRhf/BkVQbUzbrspBwBY83h+fDNs=",
        "consensus_param_updates": {
            "block": {
                "max_bytes": "22020096",
                "max_gas": "-1"
            },
            "evidence": {
                "max_age_duration": "1814400000000000",
                "max_age_num_blocks": "279138"
            },
            "validator": {
                "pub_key_types": [
                    "ed25519"
                ]
            }
        },
        "finalize_block_events": [
            {
                "attributes": [
                    {
                        "index": true,
                        "key": "spender",
                        "value": "akash17xpfvakm2amg962yls6f84z3kell8c5lazw8j8"
                    }
                ],
                "type": "coin_spent"
            }
        ],
        "height": "28742282",
        "txs_results": [
            {
                "code": 0,
                "codespace": "",
                "data": "Ei4KLC9jb3Ntd2FzbS53YXNtLnYxLk1zZ0V4ZWN1dGVDb250cmFjdFJlc3BvbnNl",
                "events": [
                    {
                        "attributes": [
                            {
                                "index": true,
                                "key": "spender",
                                "value": "akash1qafvet3v5nlkqdrlrkayy0eenq80aprqvj6nap"
                            }
                        ],
                        "type": "coin_spent"
                    }
                ],
                "gas_used": "302162",
                "gas_wanted": "423435",
                "info": "",
                "log": ""
            }
        ],
        "validator_updates": null
    }
}
```

## Response Fields

| Field        | Type   | Description                                    |
| ------------ | ------ | ---------------------------------------------- |
| txs\_results | array  | Per-tx ABCI results with code, gas, and events |
| height       | string | Height the results belong to                   |

## Use Cases

* **Event Indexing**: Drive an indexer from events
* **Failure Detection**: Detect failed txs via code
* **Gas Analytics**: Aggregate gas usage

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| -32603 / Internal error   | Internal error | Height out of range                               |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
