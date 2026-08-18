---
description: >-
  Example code for the getrawtransaction JSON-RPC method. Сomplete guide on how
  to use the getrawtransaction JSON-RPC method in GetBlock.io Web3
  documentation.
---

# getrawtransaction - Zcash

This method returns transaction data by txid. Verbosity 0 returns the raw serialized hex-encoded transaction bytes; verbosity 1 returns a decoded JSON object with all transaction fields, including transparent inputs/outputs and shielded pool data. Note that shielded transaction details are limited to what's on-chain — the actual amounts and recipients of shielded transactions are hidden by design.

## Parameters

| Parameter | Type    | Required | Description                                    |
| --------- | ------- | -------- | ---------------------------------------------- |
| `txid`    | string  | Yes      | 32-byte transaction ID (hex-encoded)           |
| `verbose` | integer | No       | `0` = raw hex, `1` = decoded JSON. Default `0` |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getrawtransaction",
    "params": [
        "9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b3a2b1c9d8e7f6a5b4c3d2e1f",
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
    method: 'getrawtransaction',
    params: [
        "9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b3a2b1c9d8e7f6a5b4c3d2e1f",
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
        'method': 'getrawtransaction',
        'params': [
        "9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b3a2b1c9d8e7f6a5b4c3d2e1f",
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
            "method": "getrawtransaction",
            "params": [
        "9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b3a2b1c9d8e7f6a5b4c3d2e1f",
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

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "hex": "0400008085202f89011234567890abcdef...",
        "txid": "9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b3a2b1c9d8e7f6a5b4c3d2e1f",
        "authdigest": "9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b3a2b1c9d8e7f6a5b4c3d2e1f",
        "size": 384,
        "overwintered": true,
        "version": 5,
        "versiongroupid": "26a7270a",
        "locktime": 0,
        "expiryheight": 2856382,
        "vin": [
            {
                "txid": "1cd7d1e5a67c7e4f1bb2d4c50a9c27c3b4a7f9e6b7a4919c2d5b0a6f3d9e1c4b",
                "vout": 0,
                "scriptSig": {
                    "asm": "3045022100...",
                    "hex": "483045022100..."
                },
                "value": 1.5,
                "valueSat": 150000000,
                "address": "t1c74RiTicVSKthLmn2c4WFLJdLDs5sQ4X6",
                "sequence": 4294967295
            }
        ],
        "vout": [
            {
                "value": 1.49999,
                "valueZat": 149999000,
                "n": 0,
                "scriptPubKey": {
                    "asm": "OP_DUP OP_HASH160 abc... OP_EQUALVERIFY OP_CHECKSIG",
                    "hex": "76a914abc...88ac",
                    "reqSigs": 1,
                    "type": "pubkeyhash",
                    "addresses": [
                        "t1KG5xEeywXupj9dJHTvHzT5eN4RvpxaAJc"
                    ]
                }
            }
        ],
        "vShieldedSpend": [],
        "vShieldedOutput": [],
        "orchard": {
            "actions": [],
            "valueBalance": 0.0,
            "valueBalanceZat": 0,
            "flags": {
                "enableSpends": true,
                "enableOutputs": true
            }
        },
        "bindingSig": "",
        "blockhash": "00000000018bb43cadf5b4c8a2e7f9d0e1a6b3c8d5e4f2a1b0c9d8e7f6a5b4c3",
        "confirmations": 1,
        "time": 1747852800,
        "blocktime": 1747852800
    }
}
```

## Response Parameters

| Parameter              | Type            | Description                                                   |
| ---------------------- | --------------- | ------------------------------------------------------------- |
| `hex`                  | string          | Serialized transaction bytes, hex-encoded                     |
| `txid`                 | string          | Transaction ID                                                |
| `authdigest`           | string          | Authorization digest (post-NU5, ZIP 244)                      |
| `size`                 | integer         | Serialized transaction size in bytes                          |
| `overwintered`         | boolean         | Whether this is an Overwintered (post-Sapling) transaction    |
| `version`              | integer         | Transaction version                                           |
| `versiongroupid`       | string          | Version group identifier                                      |
| `locktime`             | integer         | Transaction locktime                                          |
| `expiryheight`         | integer         | Height at which the transaction expires (0 = no expiry)       |
| `vin`                  | array of object | Transparent inputs                                            |
| `vout`                 | array of object | Transparent outputs                                           |
| `vShieldedSpend`       | array of object | Sapling shielded spends (empty for non-Sapling transactions)  |
| `vShieldedOutput`      | array of object | Sapling shielded outputs (empty for non-Sapling transactions) |
| `orchard.actions`      | array of object | Orchard actions (empty for non-Orchard transactions)          |
| `orchard.valueBalance` | number          | Net Orchard value balance in ZEC                              |
| `orchard.flags`        | object          | Orchard action flags (`enableSpends`, `enableOutputs`)        |
| `bindingSig`           | string          | Sapling binding signature                                     |
| `blockhash`            | string          | Hash of the block containing this transaction                 |
| `confirmations`        | integer         | Number of confirmations                                       |
| `time`                 | integer         | Unix timestamp of the containing block                        |
| `blocktime`            | integer         | Same as `time`                                                |

## Use Cases

* **Explorer Transaction Detail**: Full transaction data for an explorer's transaction detail page
* **Wallet Post-Submit Verification**: Poll after `sendrawtransaction` to confirm inclusion and read final block placement
* **Shielded Transaction Analysis**: Detect Sapling vs Orchard usage patterns by inspecting `vShieldedSpend`/`vShieldedOutput` vs `orchard.actions`
* **Transparent-Pool Analytics**: Iterate `vin` and `vout` to track transparent-pool flows for compliance or research

## Error Handling

| Error Code | Message                                    | Description                                                 |
| ---------- | ------------------------------------------ | ----------------------------------------------------------- |
| -5         | No information available about transaction | Transaction ID not found in the chain or mempool            |
| -32602     | Invalid params                             | Transaction ID is malformed or verbose parameter is invalid |
| -32603     | Internal error                             | Node failed to retrieve transaction data                    |
