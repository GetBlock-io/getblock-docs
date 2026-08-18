---
description: >-
  Example code for the getblock JSON-RPC method. Сomplete guide on how to use
  the getblock JSON-RPC method in GetBlock.io Web3 documentation.
---

# getblock - Zcash

This method returns block data by hash or height. Post-Ironwood activation, the verbose (verbosity 1 and 2) response includes the extended note commitment tree information: subtree roots, tree metadata, and the `nTx` field (per-block transaction count) added in recent Zebra releases. Verbosity 0 returns hex-encoded serialized block data; verbosity 1 returns block header + transaction hashes; verbosity 2 returns full transaction objects.

## Parameters

| Parameter             | Type    | Required | Description                                                                           |
| --------------------- | ------- | -------- | ------------------------------------------------------------------------------------- |
| `blockhash_or_height` | string  | Yes      | Block hash (hex string) or height (as a string, e.g. `"2856342"`)                     |
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
        "2856342",
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
        "2856342",
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
        "2856342",
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
        "2856342",
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
    "id": "getblock.io",
    "result": {
        "hash": "00000000015aa1af4cc4e77fd576ff5b011a9ab42170bbcc008b2b9a84648165",
        "confirmations": 564159,
        "size": 38012,
        "height": 2856342,
        "version": 4,
        "merkleroot": "b0c2c104947f0af676d1b3cc14b92b1598ea9aab9e9d8584784f3d2dc096b601",
        "blockcommitments": "13ba0a42b848545899365fb6570c300c4b3985d5d81b01be0788a206b7c84913",
        "finalsaplingroot": "05dd6a34f18e3e9d5972603261b31fd620b4573af1808000d4b14144b6165157",
        "finalorchardroot": "5368a95be8eb8153ba6a1d401f52e196805a86c9b9a0d6fcd8ff9cb10a8af41c",
        "nTx": 9,
        "tx": [
            "d5875bd53b0ed5e4adf7a5925e3dc8101a1804ad3697570f339ec9664541533e",
            "31c167575b128799eb3765faa5a5fd76bc0308469e76ddd05b9e801850986495",
            "4c61aab0107892d06d193174a4c8bf5d1e91c98922805038b21bb40a09354e14",
            "ba02c2861e431f35df75ee9a380afbe841b5f6bf4569876f1af710c2c17ff05b",
            "d9878da354535749cdd2b8ac83a1f46ea02312ac2d3a10164544b0228626c94a",
            "ac3ad834fcde84188ed11a0e3c6bcedff386076aacc24319690bb1ae346ce9b7",
            "4ee2ce8066b1b39c9c80facb6d1e585ee7719ccc495b1886aae0a6d3444b8350",
            "9e8dea2666746901e6f2116c7ab9cb12d632bb6d0c1776029b47c21c0d4717d9",
            "aba8e40e53c13c0780b0b8618622b10559c960dd5b4d71cbe7daf2fb08560f15"
        ],
        "time": 1742155675,
        "nonce": "a68e00800000000000000000006d000000000000000000000000000099b5b3db",
        "solution": "0008dab73acb0105e5ae23bb3d6e04e0f5b0fae72633a5a4136c91aff760e6062801f2a681c8667c887907aad2dc82b17545a33cc4a764dd8329e4d3ddc02736d8b3d485abd34dcfa149c68b5128ea9c773767bb15d6e5a96861611d729b5455505dcbfa462d9fe35f2329bb58f909df996c7ab349abb94f4cfd10356e651d77a548d961d1f7814084a86274249287f0dd335e5f1c947af898810bd9929bc3557b8e87243b1e416e02111547ab5ff12dbe442507b6da61d94a2d5bc900034e486b1dc9e4375d4710ca50ed59818801562b2350125599ced898676f47851b1fafe2e712909911a7538303039d1e8dcb3be698a0f0f89efe3e58386e5c1c1577d84e9040c7722ad4b0d1b3981701b399f60244f15329eb67bcd946b234da7fb375727b95b720572789a50c758ac9cd944624c5c4b162d57d57f7264957213fac0f1f5f2eff44aa4088d311d2a9dd3ee1e700ed70b0ff489ef4f7de7452ab750df2a006fd13ab40813554de24fa55ddcf8725673afb12c9965d69f015587729c7d2063dc935f1afc6714155db3713986a18b4e7974e98513b7af502345c66385d77c5ff5e710255a05c1754391f58eeb0eb0adc993f074039155615dfb204a946ade52cedf173a94c3475b924fe8e350ae2141eaf5790971d0041e4961e5b8668b55fd9f9439d23c128e22ca1fe3e0a31c66da93b5e8bdc8baa01a72afbf4f75047beaa666abbbe4d56645ef3e187042f4b29b09574474c62c4d2df7710b5c6fd9295c8093803d54c8d83bd9ab0f12f255dc0305460f8b73e4121aaac1dd6c70d9a5965249ef85ed24fbd384e2c038716bd4807501676739542a37baaf752d6dc19a00d8fba7cf9647a398c53b2f286d62be4cff3beef2e04dbb4f47a63becb55eb72432f3898de8ef33ab3450b45855bf6f57c7bf6744320a17e3dc4f0381261320022334adcf3768bb1b5e36d40343c831b66dbde09198a9be909fb2a5beb55737597a271020401f5e3df00ca89a1d3854332484fc3c21c6e39a928039d5a013a331c19ace41faf8af258be035868de4e3133fc060073f8ffd6c517482dedc67d7ef4d95e6f83982f1533ed8a6a085f9b8f623f245579c60d72f813f918b8127e8143bb99791306eeb72ca1c5dd8f47a85fd846244c7eac9e2394b53a269280eaff718d49dc15383804443baa12849ec108fea3714230df58fb1cc9cf0b1e607cd1d1520a61fa71836100a870430ba979199b081d2be224d136fbd93f83d11fa2aae9d94835900c66b393ba0722e137558b5a7fecf82bc2d4483cc9290905d2afde047ebbd6e20673e86d90bacefed984a8155eea9ce23f1aaffb13c2938d3049aca7b7d32f3b0c3005024b6b3f71b3cb21beb9bc893586b9b5ff6d29286f5d5d512881c89284641258a1424ff8d45ff9011820a65e196c111c2a4169334621e1b0653f38751b32de404a57c1472cc5798d635d0742ab90db85d907d855a145df6d59cd6164f6f9f952fddf9cf45a9b07f87f4bac83713fc604e24d31951c78aeea2c6608019e798548a1786f16c6a8a7d4fd77fac4fd3bceed0f6d479be094bf6b288f0a0744fe372681dc58968d13fa2390eadb0aa18509d28c39287586478ed6d61e249ccfedb98ad9c2e3f6f4a21c3e62a54ea471b6371147bd033a92f1594a15331bef3089f573759cb4981cc7b8fcc7cc92126878f4c6d0a7e3e5d08af103cb2b1b7487201a601bacda0a9daad4fa5b9f623ea9ac3a76f74bae9ac9030df643bb8edf18e5717b7430c52070fb201e0e259fe7a3f238cf61cd5e35b9dda57b36904ff7246873cbda8f13e1dcad4c9a66583636bc2c447cf4a8cf4eb3a92443b17d674294ee7b6cebac388597352fe04e218c2a040f2271d97e197734919e",
        "bits": "1c021126",
        "difficulty": 64933902.09056415,
        "chainSupply": {
            "chainValue": 15952667.3530448,
            "chainValueZat": 1595266735304480,
            "monitored": true
        },
        "valuePools": [
            {
                "id": "transparent",
                "chainValue": 13400654.8473006,
                "chainValueZat": 1340065484730060,
                "monitored": true,
                "valueDelta": 41.9797,
                "valueDeltaZat": 4197970000
            },
            {
                "id": "sprout",
                "chainValue": 25824.3616251,
                "chainValueZat": 2582436162510,
                "monitored": true,
                "valueDelta": 0.0,
                "valueDeltaZat": 0
            },
            {
                "id": "sapling",
                "chainValue": 860008.20094153,
                "chainValueZat": 86000820094153,
                "monitored": true,
                "valueDelta": -0.0003,
                "valueDeltaZat": -30000
            },
            {
                "id": "orchard",
                "chainValue": 1641815.63067757,
                "chainValueZat": 164181563067757,
                "monitored": true,
                "valueDelta": -40.6044,
                "valueDeltaZat": -4060440000
            },
            {
                "id": "lockbox",
                "chainValue": 24364.3125,
                "chainValueZat": 2436431250000,
                "monitored": true,
                "valueDelta": 0.1875,
                "valueDeltaZat": 18750000
            },
            {
                "id": "ironwood",
                "chainValue": 0.0,
                "chainValueZat": 0,
                "monitored": false,
                "valueDelta": 0.0,
                "valueDeltaZat": 0
            }
        ],
        "trees": {
            "sapling": {
                "size": 73749444
            },
            "orchard": {
                "size": 49024972
            }
        },
        "previousblockhash": "0000000000c5708ede27ddabdae9bd24d1d05791ac740cda22c8ab2c8d650dc8",
        "nextblockhash": "0000000002065f766e11656ee6d6e5c540c24c2c943a8d9f03809ae8d93ea199"
    }
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
| `authdataroot`       | string                    | Authorizing data commitment root (post-NU5)                             |
| `finalsaplingroot`   | string                    | Final Sapling note commitment tree state after this block               |
| `chainhistoryroot`   | string                    | Chain history commitment (post-Heartwood)                               |
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
