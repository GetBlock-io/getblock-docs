---
description: >-
  Example code for the kate_queryProof JSON-RPC method. Complete guide on how to
  use kate_queryProof JSON-RPC in GetBlock Web3 documentation.
---

# kate\_queryProof - Avail

Returns KZG opening proofs for the requested cells (row/column positions) of a block's data matrix. Light clients verify these proofs against the block header's data-root commitment during data-availability sampling.

## Parameters

| Parameter | Type   | Required | Description                        |
| --------- | ------ | -------- | ---------------------------------- |
| cells     | array  | Yes      | Array of {row, col} cells to prove |
| at        | string | Yes      | Block hash                         |

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
    "method": "kate_queryProof",
    "params": [[{"row": 0, "col": 0}, {"row": 1, "col": 2}], "0x8a1e6d2f0b7c4a9e3d5f1b8c2a6e4d0f9b3c7a5e1d8f2b6c4a0e9d3f7b1c5a2e6"]
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'kate_queryProof', params: [[{"row": 0, "col": 0}, {"row": 1, "col": 2}], "0x8a1e6d2f0b7c4a9e3d5f1b8c2a6e4d0f9b3c7a5e1d8f2b6c4a0e9d3f7b1c5a2e6"] }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'kate_queryProof', 'params': [[{"row": 0, "col": 0}, {"row": 1, "col": 2}], "0x8a1e6d2f0b7c4a9e3d5f1b8c2a6e4d0f9b3c7a5e1d8f2b6c4a0e9d3f7b1c5a2e6"]})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"kate_queryProof","params":[[{"row": 0, "col": 0}, {"row": 1, "col": 2}], "0x8a1e6d2f0b7c4a9e3d5f1b8c2a6e4d0f9b3c7a5e1d8f2b6c4a0e9d3f7b1c5a2e6"]})).send().await?.json::<Value>().await?;
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
    "result": [
        {
            "scalar": "0x...",
            "proof": "0x..."
        },
        {
            "scalar": "0x...",
            "proof": "0x..."
        }
    ]
}
```

## Response Fields

| Field  | Type  | Description                                                        |
| ------ | ----- | ------------------------------------------------------------------ |
| result | array | Per-cell {scalar, proof}: the cell value and its KZG opening proof |

## Use Cases

* **Data Availability Sampling**: Verify sampled cells against the commitment
* **Light Clients**: Prove data is available without downloading the block
* **Bridges**: Provide DA proofs to external verifiers

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| -32602 / Invalid params   | Invalid params | A parameter was malformed                         |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
