---
description: >-
  Example code for the kate_queryDataProof JSON-RPC method. Complete guide on
  how to use kate_queryDataProof JSON-RPC in GetBlock Web3 documentation.
---

# kate\_queryDataProof - Avail

Returns a Merkle proof that a submitted data transaction is included in a block's data root, along with the data, blob, and bridge roots. Rollups and bridges use this to prove on Ethereum that their data was made available on Avail.

## Parameters

| Parameter          | Type    | Required | Description                                           |
| ------------------ | ------- | -------- | ----------------------------------------------------- |
| transaction\_index | integer | Yes      | Index of the data-submission transaction in the block |
| at                 | string  | Optional | Block hash; omit for the latest block                 |

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
    "method": "kate_queryDataProof",
    "params": [1, "0x8a1e6d2f0b7c4a9e3d5f1b8c2a6e4d0f9b3c7a5e1d8f2b6c4a0e9d3f7b1c5a2e6"]
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'kate_queryDataProof', params: [1, "0x8a1e6d2f0b7c4a9e3d5f1b8c2a6e4d0f9b3c7a5e1d8f2b6c4a0e9d3f7b1c5a2e6"] }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'kate_queryDataProof', 'params': [1, "0x8a1e6d2f0b7c4a9e3d5f1b8c2a6e4d0f9b3c7a5e1d8f2b6c4a0e9d3f7b1c5a2e6"]})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"kate_queryDataProof","params":[1, "0x8a1e6d2f0b7c4a9e3d5f1b8c2a6e4d0f9b3c7a5e1d8f2b6c4a0e9d3f7b1c5a2e6"]})).send().await?.json::<Value>().await?;
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
        "dataProof": {
            "roots": {
                "dataRoot": "0x...",
                "blobRoot": "0x...",
                "bridgeRoot": "0x..."
            },
            "proof": [
                "0x...",
                "0x..."
            ],
            "numberOfLeaves": 4,
            "leafIndex": 1,
            "leaf": "0x..."
        },
        "message": null
    }
}
```

## Response Fields

| Field                    | Type    | Description                                 |
| ------------------------ | ------- | ------------------------------------------- |
| dataProof.roots.dataRoot | string  | Data root committed in the block header     |
| dataProof.proof          | array   | Merkle proof from the leaf to the data root |
| dataProof.leaf           | string  | Hash of the submitted data blob             |
| dataProof.leafIndex      | integer | Index of the leaf in the data-root tree     |

## Use Cases

* **Bridging**: Prove data availability to a bridge on Ethereum
* **Rollups**: Submit an availability proof for a batch
* **Verification**: Confirm a blob was included in the data root

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| -32602 / Invalid params   | Invalid params | A parameter was malformed                         |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
