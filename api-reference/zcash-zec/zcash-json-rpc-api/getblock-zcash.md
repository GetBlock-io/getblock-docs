---
description: >-
  Example code for the getblock JSON-RPC method. Complete guide on how to use
  the getblock JSON-RPC method in GetBlock.io Web3 documentation.
---

# getblock - Zcash

This method returns block data by hash or height. Post-Ironwood activation, the verbose (verbosity 1 and 2) response includes the extended note commitment tree information: subtree roots, tree metadata, and the `nTx` field (per-block transaction count) added in recent Zebra releases. Verbosity 0 returns hex-encoded serialized block data; verbosity 1 returns block header + transaction hashes; verbosity 2 returns full transaction objects.

## Parameters

| Parameter             | Type    | Required | Description                                                                           |
| --------------------- | ------- | -------- | ------------------------------------------------------------------------------------- |
| `blockhash_or_height` | string  | Yes      | Block hash (hex string) or height (as a string, e.g. `"3487790"`)                     |
| `verbosity`           | integer | No       | `0` = raw hex, `1` = summary + tx hashes, `2` = full transaction objects. Default `1` |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getblock",
    "params": [
        "3487790",
        1
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
    method: 'getblock',
    params: [
        "3487790",
        1
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
        'method': 'getblock',
        'params': [
        "3487790",
        1
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
            "method": "getblock",
            "params": [
        "3487790",
        1
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
        "size": 216827,
        "height": 3487790,
        "version": 4,
        "merkleroot": "2134e7beb96aac05105adbbe8ef04fab84739f8a90e22e73e44e6add4812ed35",
        "blockcommitments": "50f7a4921c16db984372e06ebc91e971d9412926d50cea958e71d496fc58682e",
        "finalsaplingroot": "59d2ea590d75d202eaa4690d627c21c56b59474991e197725804b25b8206133c",
        "finalorchardroot": "c024476612f29e6229bf7d6d260a506e227fd1673e2770a530ae5881c49f2428",
        "nTx": 32,
        "tx": [
            "bdfca4416851a17616c5900903ef41907429f62302731db3517e9c5815287b42",
            "80d21f43485fb5aed2bcb011a00fb70776b2f6b4ba05722dbf2dce6f53fc410f"
        ],
        "time": 1789741929,
        "nonce": "a341002a000000000000000000000000000000000000000000000000d094f930",
        "solution": "00ede59b1b273717f4ec030131a4bb48c4a0edc3181219faec7dd00dc5bf7fb19762f52384676b04bceb01881d81d606...",
        "bits": "1b762f01",
        "difficulty": 290731287.69865835,
        "chainSupply": {
            "chainValue": 16939304.8530448,
            "chainValueZat": 1693930485304480,
            "monitored": true
        },
        "valuePools": [
            {
                "id": "transparent",
                "chainValue": 11959062.86381575,
                "chainValueZat": 1195906286381575,
                "monitored": true,
                "valueDelta": 19.24234538,
                "valueDeltaZat": 1924234538
            },
            {
                "id": "sprout",
                "chainValue": 22481.46711837,
                "chainValueZat": 2248146711837,
                "monitored": true,
                "valueDelta": 0.0,
                "valueDeltaZat": 0
            }
        ],
        "trees": {
            "sapling": {
                "size": 73960334
            },
            "orchard": {
                "size": 50462480
            },
            "ironwood": {
                "size": 392433
            }
        },
        "previousblockhash": "000000000007b84c009ac090a37dd74b23bcfa4aeb6620960c7d0ee3f5fb664a",
        "nextblockhash": "00000000000aa124983da79c1e3bd8524b5b76da7b43bb1a88edd93ac5f61331"
    },
    "id": "getblock.io"
}
```
{% endcode %}

## Response Parameters

| Parameter            | Type                      | Description                                                             |
| -------------------- | ------------------------- | ----------------------------------------------------------------------- |
| `hash`               | string                    | Block hash                                                              |
| `confirmations`      | integer                   | Number of confirmations (1 = chain tip; -1 = orphaned)                  |
| `size`               | integer                   | Serialized block size in bytes                                          |
| `height`             | integer                   | Block height                                                            |
| `version`            | integer                   | Block version number                                                    |
| `merkleroot`         | string                    | Merkle root of the transactions in the block                            |
| `blockcommitments`   | string                    | Block commitments hash (post-Heartwood, ZIP 244)                        |
| `finalsaplingroot`   | string                    | Final Sapling note commitment tree state after this block               |
| `tx`                 | array of string or object | Transaction IDs (verbosity 1) or full transaction objects (verbosity 2) |
| `trees.sapling.size` | integer                   | Size of the Sapling note commitment tree at this block                  |
| `trees.orchard.size` | integer                   | Size of the Orchard note commitment tree at this block                  |
| `nTx`                | integer                   | Number of transactions in the block (verbose responses only)            |
| `time`               | integer                   | Unix timestamp of block production                                      |
| `nonce`              | string                    | Block nonce (hex)                                                       |
| `solution`           | string                    | Equihash solution (hex)                                                 |
| `bits`               | string                    | Compact difficulty target                                               |
| `difficulty`         | number                    | Difficulty of this block                                                |
| `previousblockhash`  | string                    | Hash of the parent block                                                |
| `nextblockhash`      | string                    | Hash of the next block (empty at the chain tip)                         |

## Use Cases

* **Explorer Block Detail Pages**: Full block metadata for an explorer's block detail view
* **Indexer Ingestion**: Verbosity 2 loads every transaction in the block in one call
* **Shielded-Pool State Snapshots**: Read `finalsaplingroot` and `trees.*` to reconstruct pool state at a specific block
* **Reorg Handling**: Check `confirmations` — a value of `-1` indicates the block was orphaned
* **Transaction Count Metrics**: Post-Ironwood: use `nTx` for cheap per-block transaction-count aggregation

## Error Handling

| Error Code | Message         | Description                                               |
| ---------- | --------------- | --------------------------------------------------------- |
| -8         | Block not found | Requested hash or height doesn't exist                    |
| -32602     | Invalid params  | Hash or height is malformed, or verbosity is out of range |
| -32603     | Internal error  | Node failed to retrieve the block                         |
