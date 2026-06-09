---
description: >-
  Example code for the get_consensus JSON-RPC method. Complete guide on how to
  use get_consensus JSON-RPC in GetBlock Web3 documentation.
---

# get\_consensus - Nervos

Returns chain consensus parameters — protocol-level constants such as genesis hash, max block bytes, epoch duration target, primary issuance per epoch, and active hardforks.

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "get_consensus",
    "params": [],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "get_consensus",
    "params": [],
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers: {
        'Content-Type': 'application/json'
    },
    data: data
};

axios(config)
    .then(response => console.log(JSON.stringify(response.data, null, 2)))
    .catch(error => console.log(error));
```
{% endtab %}

{% tab title="Python (Requests)" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "get_consensus",
    "params": [],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endtab %}

{% tab title="Rust (Reqwest)" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let payload = json!({
        "jsonrpc": "2.0",
        "method": "get_consensus",
        "params": [],
        "id": "getblock.io"
});

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&payload)
        .send()
        .await?;

    let result: serde_json::Value = response.json().await?;
    println!("{:#?}", result);

    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "id": "ckb",
        "genesis_hash": "0x92b197aa1fba0f63633922c61c92375c9c074a93e85963554f5499fe1450d0e5",
        "dao_type_hash": "0x82d76d1b75fe2fd9a27dfbaa65a039221a380d76c926f378d3f81cf3e7e13f2e",
        "secp256k1_blake160_sighash_all_type_hash": "0x709f3fda12f561cfacf92273c57a98fede188a3f1a59b1f888d113f9cce08649",
        "secp256k1_blake160_multisig_all_type_hash": "0x5c5069eb0857efc65e1bca0c07df34c31663b3622fd3876c876320fc9634e2a8",
        "initial_primary_epoch_reward": "0x71afd498d000",
        "secondary_epoch_reward": "0x37d0790",
        "max_uncles_num": "0x2",
        "orphan_rate_target": {
            "numer": "0x28",
            "denom": "0x3e8"
        },
        "epoch_duration_target": "0x3840",
        "tx_proposal_window": {
            "closest": "0x2",
            "farthest": "0xa"
        },
        "proposer_reward_ratio": {
            "numer": "0x4",
            "denom": "0xa"
        },
        "cellbase_maturity": "0x80000000000000a",
        "median_time_block_count": "0x25",
        "max_block_cycles": "0xd09dc300",
        "max_block_bytes": "0x91c08",
        "block_version": "0x0",
        "tx_version": "0x0",
        "type_id_code_hash": "0x00000000000000000000000000000000000000000000000000545950455f4944",
        "max_block_proposals_limit": "0x5dc",
        "primary_epoch_reward_halving_interval": "0x2238",
        "permanent_difficulty_in_dummy": false,
        "hardfork_features": [
            {
                "rfc": "0028",
                "epoch_number": "0x1526"
            }
        ],
        "softforks": {}
    }
}
```
{% endcode %}

## Response Parameters

| Field                          | Type                     | Description                                                                                  |
| ------------------------------ | ------------------------ | -------------------------------------------------------------------------------------------- |
| result.id                      | string                   | Chain identifier — `"ckb"` for mainnet, `"ckb_testnet"` for testnet                          |
| result.genesis\_hash           | H256                     | Genesis block hash                                                                           |
| result.max\_block\_bytes       | Uint64                   | Maximum block size in bytes                                                                  |
| result.max\_block\_cycles      | Uint64                   | Maximum CKB-VM cycles per block                                                              |
| result.epoch\_duration\_target | Uint64                   | Target epoch duration in milliseconds (\~4 hours)                                            |
| result.cellbase\_maturity      | EpochNumberWithFraction  | Number of epochs after which cellbase outputs become spendable                               |
| result.tx\_proposal\_window    | object                   | 2-step commit window — `closest` is min blocks between propose and commit, `farthest` is max |
| result.hardfork\_features      | array of HardforkFeature | Activated hardforks with their RFC numbers and activation epochs                             |

## Use Cases

* Verifying network identity (`id` field — `"ckb"` for mainnet)
* SDKs that need protocol-level constants
* Detecting hardfork activation over time
* Computing maximum transaction size or cycle limits

## Error Handling

| Status Code | Error Message     | Cause                                                |
| ----------- | ----------------- | ---------------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                  |
| -32602      | Invalid params    | Request parameters are missing or malformed          |
| -32601      | Method not found  | Method does not exist or is not enabled on this node |
| -32603      | Internal error    | Server-side error while processing the request       |
| 429         | Too Many Requests | Rate limit exceeded for your plan                    |

## SDK Integration

{% tabs %}
{% tab title="@ckb-lumos/lumos" %}
```javascript
import { RPC } from '@ckb-lumos/rpc';

const rpc = new RPC('https://go.getblock.io/<ACCESS-TOKEN>/');

// Lumos provides typed methods on `rpc` (e.g. rpc.getTipHeader(), rpc.getBlock(hash)).
// For raw access to any CKB RPC method:
const result = await rpc.send('get_consensus', []);
console.log(result);
```
{% endtab %}

{% tab title="ckb-sdk-python / requests" %}
```python
# Using the official CKB SDK Python (ckb-sdk-python).
# For methods not yet wrapped, fall back to raw requests.
import requests

response = requests.post('https://go.getblock.io/<ACCESS-TOKEN>/', json={
    'jsonrpc': '2.0',
    'id': 'getblock.io',
    'method': 'get_consensus',
    'params': []
}})
print(response.json())
```
{% endtab %}
{% endtabs %}
