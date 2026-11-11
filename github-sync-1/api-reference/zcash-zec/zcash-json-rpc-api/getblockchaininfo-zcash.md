---
description: >-
  Example code for the getblockchaininfo JSON-RPC method. Complete guide on how
  to use the getblockchaininfo JSON-RPC method in GetBlock.io Web3
  documentation.
---

# getblockchaininfo - Zcash

This method returns comprehensive blockchain state: chain name, current best height and hash, activated network upgrades, transparent and shielded pool balances, and estimated header count. It's the canonical endpoint for wallets and indexers to understand the current chain state in one call.

## Parameters

This method takes no parameters.

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getblockchaininfo",
    "params": [],
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
    method: 'getblockchaininfo',
    params: [],
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
        'method': 'getblockchaininfo',
        'params': [],
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
            "method": "getblockchaininfo",
            "params": [],
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
        "chain": "main",
        "blocks": 3487829,
        "headers": 3487829,
        "difficulty": 283406385.74557334,
        "verificationprogress": 1.0,
        "chainwork": 0,
        "pruned": false,
        "size_on_disk": 278526085121,
        "commitments": 0,
        "bestblockhash": "000000000044b7b0276f3baa73a690467eaff42379eaeeb197a983c6f1408ce1",
        "estimatedheight": 3487829,
        "chainSupply": {
            "chainValue": 16939365.7905448,
            "chainValueZat": 1693936579054480,
            "monitored": true
        },
        "valuePools": [
            {
                "id": "transparent",
                "chainValue": 11959273.50520267,
                "chainValueZat": 1195927350520267,
                "monitored": true
            },
            {
                "id": "sprout",
                "chainValue": 22481.46711837,
                "chainValueZat": 2248146711837,
                "monitored": true
            }
        ],
        "upgrades": {
            "5ba81b19": {
                "name": "Overwinter",
                "activationheight": 347500,
                "status": "active"
            },
            "76b809bb": {
                "name": "Sapling",
                "activationheight": 419200,
                "status": "active"
            },
            "2bb40e60": {
                "name": "Blossom",
                "activationheight": 653600,
                "status": "active"
            },
            "f5b9230b": {
                "name": "Heartwood",
                "activationheight": 903000,
                "status": "active"
            },
            "e9ff75a6": {
                "name": "Canopy",
                "activationheight": 1046400,
                "status": "active"
            },
            "c2d6d0b4": {
                "name": "NU5",
                "activationheight": 1687104,
                "status": "active"
            },
            "c8e71055": {
                "name": "NU6",
                "activationheight": 2726400,
                "status": "active"
            },
            "4dec4df0": {
                "name": "NU6.1",
                "activationheight": 3146400,
                "status": "active"
            },
            "5437f330": {
                "name": "NU6.2",
                "activationheight": 3364600,
                "status": "active"
            },
            "37a5165b": {
                "name": "NU6.3",
                "activationheight": 3428143,
                "status": "active"
            }
        },
        "consensus": {
            "chaintip": "37a5165b",
            "nextblock": "37a5165b"
        }
    },
    "id": "getblock.io"
}
```
{% endcode %}

## Response Parameters

| Parameter                        | Type            | Description                                                        |
| -------------------------------- | --------------- | ------------------------------------------------------------------ |
| `chain`                          | string          | Chain name — `main` or `test`                                      |
| `blocks`                         | integer         | Current best-known chain tip height                                |
| `headers`                        | integer         | Number of validated headers                                        |
| `bestblockhash`                  | string          | Hash of the current chain tip                                      |
| `difficulty`                     | number          | Current proof-of-work difficulty multiplier                        |
| `verificationprogress`           | number          | Sync progress as a fraction from 0.0 to 1.0                        |
| `chainwork`                      | string          | Total accumulated proof-of-work as a hex string                    |
| `pruned`                         | boolean         | Whether the node has pruned historical blocks                      |
| `size_on_disk`                   | integer         | Total disk space used by the node's chain state                    |
| `commitments`                    | integer         | Total note commitments across all shielded pools                   |
| `valuePools`                     | array of object | Per-pool balance breakdown (transparent, sprout, sapling, orchard) |
| `valuePools[].id`                | string          | Pool identifier (`transparent`, `sprout`, `sapling`, `orchard`)    |
| `valuePools[].chainValue`        | number          | Pool balance in ZEC                                                |
| `valuePools[].chainValueZat`     | integer         | Pool balance in zatoshi (1 ZEC = 10^8 zat)                         |
| `upgrades`                       | object          | Map of network upgrade branch ID → activation info                 |
| `upgrades.<id>.name`             | string          | Human-readable upgrade name                                        |
| `upgrades.<id>.activationheight` | integer         | Height at which the upgrade activated                              |
| `upgrades.<id>.status`           | string          | `active`, `pending`, `disabled`, or `failed`                       |
| `consensus.chaintip`             | string          | Consensus branch ID at the current tip                             |
| `consensus.nextblock`            | string          | Consensus branch ID for the next block                             |
| `estimatedheight`                | integer         | Estimated chain tip height based on peer headers                   |

## Use Cases

* **Wallet Sync State Detection**: Read `verificationprogress` and compare `blocks` to `estimatedheight` to determine if the node is synced
* **Network Upgrade Awareness**: Check the `upgrades` map to determine which consensus rules apply — critical for transaction construction after a network upgrade
* **Value Pool Analytics**: Track shielded-pool balances across Sapling and Orchard for research and monitoring
* **Consensus Branch Detection**: Read the `consensus.nextblock` branch ID for transaction construction — required in transaction signature hashes post-NU5
* **Storage Capacity Planning**: Monitor `size_on_disk` on self-hosted nodes for infrastructure sizing

## Error Handling

| Error Code | Message        | Description                                        |
| ---------- | -------------- | -------------------------------------------------- |
| -32603     | Internal error | Node failed to compile the blockchain info payload |
