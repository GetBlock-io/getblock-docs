---
description: >-
  Example code for the z_gettreestate JSON-RPC method. Complete guide on how to
  use the z_gettreestate JSON-RPC method in GetBlock.io Web3 documentation.
---

# z\_gettreestate - Zcash

This method returns the Sapling and Orchard note commitment tree state at a specific block. Post-Ironwood activation (NU6.3), the response includes extended metadata: subtree roots and skip-list hashes used by wallets synchronizing shielded balances and by light clients validating shielded proofs. Critical for zk-SNARK proof construction and shielded-note witness path derivation.

## Parameters

| Parameter | Type   | Required | Description                        |
| --------- | ------ | -------- | ---------------------------------- |
| `block`   | string | Yes      | Block hash or height (as a string) |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "z_gettreestate",
    "params": [
        "3487790"
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
    method: 'z_gettreestate',
    params: [
        "3487790"
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
        'method': 'z_gettreestate',
        'params': [
        "3487790"
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
            "method": "z_gettreestate",
            "params": [
        "3487790"
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
        "height": 3487790,
        "time": 1789741929,
        "sapling": {
            "commitments": {
                "finalRoot": "59d2ea590d75d202eaa4690d627c21c56b59474991e197725804b25b8206133c",
                "finalState": "01d1d174de05671a9fd12f5714f1063af786862613cc6fe7517fc8a8e70d20c0550125796e51023caaf05e142eaef848..."
            }
        },
        "orchard": {
            "commitments": {
                "finalRoot": "c024476612f29e6229bf7d6d260a506e227fd1673e2770a530ae5881c49f2428",
                "finalState": "01d0cefdc73f6160521670d0a43e71e20e64a59ec795e69fdde328e639cc6eb12901a4bae32614a0378388240f2d90c7..."
            }
        },
        "ironwood": {
            "commitments": {
                "finalRoot": "de3ddfbb046e67ea5a6a425a892b3614a8c411703dbe604ec13a3f5ee5079d07",
                "finalState": "017f4c4ca4bff8587caf3f8cd9ba571b385d72c38fcab593a11b9694d3178f4515001f00000001d39bbe4f53f53a43d6..."
            }
        }
    },
    "id": "getblock.io"
}
```
{% endcode %}

## Response Parameters

| Parameter                        | Type    | Description                                                                                   |
| -------------------------------- | ------- | --------------------------------------------------------------------------------------------- |
| `height`                         | integer | Height of the queried block                                                                   |
| `hash`                           | string  | Block hash                                                                                    |
| `time`                           | integer | Unix timestamp of the queried block                                                           |
| `sapling.commitments.finalState` | string  | Final Sapling note commitment tree state, hex-encoded                                         |
| `orchard.commitments.finalState` | string  | Final Orchard note commitment tree state, hex-encoded                                         |

## Use Cases

* **Wallet Shielded-Balance Sync**: Light wallets fetch tree state to verify witness paths for shielded notes
* **zk-SNARK Proof Construction**: Note commitment tree state is a required input for constructing Sapling and Orchard spend proofs
* **Historical Shielded-Pool Snapshots**: Query tree state at historical blocks for research and analytics
* **Cross-Chain Attestations**: Prove shielded-pool state at a specific block for bridges or ZK verifiers

## Error Handling

| Error Code | Message         | Description                                            |
| ---------- | --------------- | ------------------------------------------------------ |
| -8         | Block not found | The requested block doesn't exist in the current chain |
| -32602     | Invalid params  | Block hash or height is malformed                      |
| -32603     | Internal error  | Node failed to compile the tree state                  |
