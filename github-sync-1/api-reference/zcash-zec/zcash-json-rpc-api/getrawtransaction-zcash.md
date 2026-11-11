---
description: >-
  Example code for the getrawtransaction JSON-RPC method. Complete guide on how
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
        "076a0119b89f661efa34e815079726b36b4cfbd0375f66af72f505c88bafde37",
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
        "076a0119b89f661efa34e815079726b36b4cfbd0375f66af72f505c88bafde37",
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
        "076a0119b89f661efa34e815079726b36b4cfbd0375f66af72f505c88bafde37",
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
        "076a0119b89f661efa34e815079726b36b4cfbd0375f66af72f505c88bafde37",
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
    "result": {
        "in_active_chain": true,
        "hex": "050000800a27a7265b16a537000000005538350000017ae69c59000000001976a91494748c0ca42e412783804af51661...",
        "height": 3487790,
        "confirmations": 41,
        "vin": [],
        "vout": [
            {
                "value": 15.03454842,
                "valueZat": 1503454842,
                "n": 0,
                "scriptPubKey": {
                    "asm": "OP_DUP OP_HASH160 94748c0ca42e412783804af516617ff4cbb49de3 OP_EQUALVERIFY OP_CHECKSIG",
                    "hex": "76a91494748c0ca42e412783804af516617ff4cbb49de388ac",
                    "reqSigs": 1,
                    "type": "pubkeyhash",
                    "addresses": [
                        "t1XQZdZMnzXBcL8yx2PR27dSNrqctgwLgux"
                    ]
                }
            }
        ],
        "vShieldedSpend": [
            {
                "cv": "a4dac0a000b061a6d0cfd4397e7cfd185a47c6b66cd60059564f6527b0cf7c76",
                "anchor": "202e3871e84a62e3401520d6322f639c23abd753e35785b0ed2bbeb7493ff036",
                "nullifier": "1025ae7d676e9c4bb199e2400dc02dc8db2484cc3ee5149894019ab42c15c0c6",
                "rk": "4e5c4da6a2df19610950ab18b3206020b431c8c7db19a03b13684aa9b9198e4f",
                "proof": "98ff19ade8b2fcf32dd9c45276457992ffd414270d541efaa93fe30a0b4f9c2573bd2effbad4d14f49857c524abbc806...",
                "spendAuthSig": "d7b953745487dc74d7c8c145b8e1f9b91dd30c831f353148eb7987015943b80a37c4856f4607ecccb9ae7019a4b5817cf4689690459d3ad4cd5ab8185c450405"
            }
        ],
        "vShieldedOutput": [
            {
                "cv": "e7c179143abebe757bc754e8ddf48b51781a048383aba6782933bf99c29c3002",
                "cmu": "55c0200de7a8c87f51e76fcc13268686f73a06f114572fd19f1a6705de74d1d1",
                "ephemeralKey": "a6911b91f80c7e1a8e5c474a83d01299dd30ce6453777fa71391c395b33c23eb",
                "encCiphertext": "3d25ce78fa6cdf60b5aa01afc80f95d0b24cf71269b8e5f309317c909e54098de51cde2dc5525f638cf9cadf0fe343ad...",
                "outCiphertext": "0b3a82f9b954491bddf193802abc97f26dd662119a11dca0d724622f13dee262d22e0e925b11559aa762b55cd50654e3...",
                "proof": "83be8ca102b7937510d460014d927a2da8abbc7f413ff4b79476baf5c09a656872f7ce55fdcd6d5222331cadda306971..."
            }
        ],
        "vjoinsplit": [],
        "bindingSig": "c0e1105f5fc66008d1da85af4238673affeb8a5ea24ec8458bcea3612bffee56591fdf565d160e9204ec1da5de1564cda0c7f3d8927d5e5c51729bc796d36b01",
        "orchard": {
            "actions": [],
            "valueBalance": 0.0,
            "valueBalanceZat": 0
        },
        "valueBalance": 15.03469842,
        "valueBalanceZat": 1503469842,
        "size": 2763,
        "time": 1789741929,
        "txid": "076a0119b89f661efa34e815079726b36b4cfbd0375f66af72f505c88bafde37",
        "authdigest": "80d7eb57f1351c0b1d924d0e80bcca682ff1988d5bd11980cf96dbeb9e61c84f",
        "overwintered": true,
        "version": 5,
        "versiongroupid": "26a7270a",
        "locktime": 0,
        "expiryheight": 3487829,
        "blockhash": "00000000000f3c0bdf81aba0bf081c5c4882975cbc0a8ce35e70ddb5f265f1b1",
        "blocktime": 1789741929
    },
    "id": "getblock.io"
}
```

## Response Parameters

| Parameter              | Type            | Description                                                   |
| ---------------------- | --------------- | ------------------------------------------------------------- |
| `hex`                  | string          | Serialized transaction bytes, hex-encoded                     |
| `txid`                 | string          | Transaction ID                                                |
| `authdigest`           | string          | Authorization digest (post-NU5, ZIP 244). Present on v5 transactions only |
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
| `bindingSig`           | string          | Sapling binding signature. Present only when the transaction has Sapling spends or outputs |
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
