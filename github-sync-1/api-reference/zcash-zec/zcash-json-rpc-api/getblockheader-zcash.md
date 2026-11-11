---
description: >-
  Example code for the getblockheader JSON-RPC method. Complete guide on how to
  use the getblockheader JSON-RPC method in GetBlock.io Web3 documentation.
---

# getblockheader - Zcash

This method returns block header data — the fixed 80-byte header plus derived fields — without the full transaction list. Significantly cheaper than `getblock` when only header data is needed, such as during header-first chain synchronization or SPV-style verification.

## Parameters

| Parameter   | Type    | Required | Description                                                                            |
| ----------- | ------- | -------- | -------------------------------------------------------------------------------------- |
| `blockhash` | string  | Yes      | Block hash (hex string)                                                                |
| `verbose`   | boolean | No       | If `true` (default), returns a JSON object; if `false`, returns raw hex-encoded header |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getblockheader",
    "params": [
        "00000000000f3c0bdf81aba0bf081c5c4882975cbc0a8ce35e70ddb5f265f1b1",
        true
    ],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'getblockheader',
    params: [
        "00000000000f3c0bdf81aba0bf081c5c4882975cbc0a8ce35e70ddb5f265f1b1",
        true
    ],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests

response = requests.post(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'getblockheader',
        'params': [
        "00000000000f3c0bdf81aba0bf081c5c4882975cbc0a8ce35e70ddb5f265f1b1",
        true
    ],
        'id': 'getblock.io'
    }
)

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
    
    let response = client
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "getblockheader",
            "params": [
        "00000000000f3c0bdf81aba0bf081c5c4882975cbc0a8ce35e70ddb5f265f1b1",
        true
    ],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;
    
    println!("Result: {}", response["result"]);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "result": {
        "hash": "00000000000f3c0bdf81aba0bf081c5c4882975cbc0a8ce35e70ddb5f265f1b1",
        "confirmations": 40,
        "height": 3487790,
        "version": 4,
        "merkleroot": "2134e7beb96aac05105adbbe8ef04fab84739f8a90e22e73e44e6add4812ed35",
        "blockcommitments": "50f7a4921c16db984372e06ebc91e971d9412926d50cea958e71d496fc58682e",
        "finalsaplingroot": "59d2ea590d75d202eaa4690d627c21c56b59474991e197725804b25b8206133c",
        "time": 1789741929,
        "nonce": "a341002a000000000000000000000000000000000000000000000000d094f930",
        "solution": "00ede59b1b273717f4ec030131a4bb48c4a0edc3181219faec7dd00dc5bf7fb19762f52384676b04bceb01881d81d606...",
        "bits": "1b762f01",
        "difficulty": 290731287.69865835,
        "previousblockhash": "000000000007b84c009ac090a37dd74b23bcfa4aeb6620960c7d0ee3f5fb664a",
        "nextblockhash": "00000000000aa124983da79c1e3bd8524b5b76da7b43bb1a88edd93ac5f61331"
    },
    "id": "getblock.io"
}
```
{% endcode %}

## Response Parameters

| Parameter           | Type    | Description                                          |
| ------------------- | ------- | ---------------------------------------------------- |
| `hash`              | string  | Block hash                                           |
| `confirmations`     | integer | Number of confirmations                              |
| `height`            | integer | Block height                                         |
| `version`           | integer | Block version                                        |
| `merkleroot`        | string  | Merkle root of transactions                          |
| `blockcommitments`  | string  | Block commitments hash (post-Heartwood)              |
| `finalsaplingroot`  | string  | Final Sapling commitment tree state after this block |
| `time`              | integer | Unix timestamp                                       |
| `nonce`             | string  | Block nonce (hex)                                    |
| `solution`          | string  | Equihash solution (hex)                              |
| `bits`              | string  | Compact difficulty target                            |
| `difficulty`        | number  | Difficulty of this block                             |
| `previousblockhash` | string  | Parent block hash                                    |
| `nextblockhash`     | string  | Next block hash (empty at chain tip)                 |

## Use Cases

* **Header-First Sync**: SPV clients download headers first to establish the canonical chain before fetching transactions
* **Reorg Detection**: Compare header hashes across polls to detect chain reorganization
* **Lightweight Block Metadata**: Get block timing, difficulty, and shielded-tree state without loading transactions
* **Chain History Reconstruction**: Iterate headers backwards from a known hash to reconstruct chain history

## Error Handling

| Error Code | Message         | Description                        |
| ---------- | --------------- | ---------------------------------- |
| -5         | Block not found | Requested hash doesn't exist       |
| -32602     | Invalid params  | Hash is malformed                  |
| -32603     | Internal error  | Node failed to retrieve the header |
