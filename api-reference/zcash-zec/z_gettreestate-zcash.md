---
description: >-
  Example code for the z_gettreestate JSON-RPC method. Сomplete guide on how to
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
        "2856342"
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
        "2856342"
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
        "2856342"
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
        "2856342"
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
    "id": "getblock.io",
    "result": {
        "hash": "00000000015aa1af4cc4e77fd576ff5b011a9ab42170bbcc008b2b9a84648165",
        "height": 2856342,
        "time": 1742155675,
        "sapling": {
            "commitments": {
                "finalRoot": "05dd6a34f18e3e9d5972603261b31fd620b4573af1808000d4b14144b6165157",
                "finalState": "011ac5f24ace7019474f2fd7fd96a398b0f7921d614a002cdd63fc497b900d406201fa6680c116f0f7f223e0fe34d5a1b14f2870be04b0dbdcdec1463b9817b052281f01c64d8808a71f679db01588eb6ae1dfe591a0dc0e6dff4911f5f1ddd7e9232b030000000001ed016c32e9d4d571dad32b8f50d2579393c89bd88d5f56cc083c414272640a2301d9a8e1f094e53ecf9cc22d514c3eab931e314920e32a88ff3d925973c26acc3401520f660be82da44d801a4e2e99fbd54a9f2fc63bac476d66640ef908d1166262019832537fdcb30b773d2b9eb52cced6ece945ed47a65db379ee890b38effe042800000161c67e558a48edf55da7420d5df1e5ef7aef8d227a2cdbc0db497592e7f0600600012ea4772527d750deb756cf526cdb1d7a50479f08a5fe7c4f016fc9ff7a23c23d0001ab58f1ebb2860e257c50350a3e1b54778b7729bdd11eacaa9213c4b5f4dbb44c00017d1ce2f0839bdbf1bad7ae37f845e7fe2116e0c1197536bfbad549f3876c3c590000013e2598f743726006b8de42476ed56a55a75629a7b82e430c4e7c101a69e9b02a011619f99023a69bb647eab2d2aa1a73c3673c74bb033c3c4930eacda19e6fd93b0000000160272b134ca494b602137d89e528c751c06d3ef4a87a45f33af343c15060cc1e0000000000"
            }
        },
        "orchard": {
            "commitments": {
                "finalRoot": "5368a95be8eb8153ba6a1d401f52e196805a86c9b9a0d6fcd8ff9cb10a8af41c",
                "finalState": "0159de9394a4d3370a68c5d977b43607942c61b9ac6fd59a54484e5670634a5c340116209a943e9a9e95ad11a00317f518d176ac0cf7df05c2bb2dde5a59b5fac92c1f011d5b4ce7672a9b4317e1cb2863db36606dfd4589e458d183bdc2c1bc79fcdc000001d999738c52ae3ecce37a2cdda4815b20c22757a869573729cd2e1b78b74c24390000012b924496a0de3fcd313e8c8380b46be1d9d4a9ce4360ee2a2b2893fadae26f1d01587d24e6addab7e77a8f4b4cbd9fd4b39353f9c705a3ab831e91b4a4ed15222301972ec404718e7c9aeb5cc02609026245f9bc40597a371bad095f41e601b2bd2f01f621d02bc76e81a0184290fbf271f7974497e83677430c083756724227915c3401c7325c39e1d0a4dd4199576028002760dc2c6c75ec654fcb9fe1c3dd8d3e121f01b3d1691aa6f8958fe94e2fa69b6b0f778b4183f8cc7539ef80c2550cfa5a2018000000000000012d113bc8f6a4f41b3963cfa0717176c2d31ce7bfae4d250a1fff5e061dd9d3250160040850b766b126a2b4843fcdfdffa5d5cab3f53bc860a3bef68958b5f066170001cc2dcaa338b312112db04b435a706d63244dd435238f0aa1e9e1598d35470810012dcc4273c8a0ed2337ecf7879380a07e7d427c7f9d82e538002bd1442978402c01daf63debf5b40df902dae98dadc029f281474d190cddecef1b10653248a234150001e2bca6a8d987d668defba89dc082196a922634ed88e065c669e526bb8815ee1b000000000000"
            }
        }
    }
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
| `sapling.skipHash`               | string  | Skip-list hash for the Sapling tree (post-Ironwood; empty in pre-Ironwood-activation windows) |
| `orchard.commitments.finalState` | string  | Final Orchard note commitment tree state, hex-encoded                                         |
| `orchard.skipHash`               | string  | Skip-list hash for the Orchard tree (post-Ironwood)                                           |

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
