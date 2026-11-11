---
description: >-
  Example code for the gettxout JSON-RPC method. Complete guide on how to use
  the gettxout JSON-RPC method in GetBlock.io Web3 documentation.
---

# gettxout - Zcash

This method returns information about a specific transparent unspent transaction output (UTXO). Returns `null` if the output has already been spent or doesn't exist. Note that this method covers **transparent outputs only** — shielded outputs are private by design and cannot be enumerated via this RPC.

## Parameters

| Parameter         | Type    | Required | Description                                                           |
| ----------------- | ------- | -------- | --------------------------------------------------------------------- |
| `txid`            | string  | Yes      | Transaction ID containing the output                                  |
| `n`               | integer | Yes      | Output index (0-based) within the transaction                         |
| `include_mempool` | boolean | No       | If `true`, include mempool transactions in the lookup. Default `true` |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "gettxout",
    "params": [
        "65951708c3d0087631c663179faf2b84e11d41b95fd56eae9dc4810e565415ee",
        1,
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
    method: 'gettxout',
    params: [
        "65951708c3d0087631c663179faf2b84e11d41b95fd56eae9dc4810e565415ee",
        1,
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
        'method': 'gettxout',
        'params': [
        "65951708c3d0087631c663179faf2b84e11d41b95fd56eae9dc4810e565415ee",
        1,
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
            "method": "gettxout",
            "params": [
        "65951708c3d0087631c663179faf2b84e11d41b95fd56eae9dc4810e565415ee",
        1,
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

```json
{
    "jsonrpc": "2.0",
    "result": {
        "bestblock": "000000000044b7b0276f3baa73a690467eaff42379eaeeb197a983c6f1408ce1",
        "confirmations": 17150,
        "value": 3.17223382,
        "scriptPubKey": {
            "asm": "OP_DUP OP_HASH160 13bbd8bc8df03a5a9cda88103f583aa8bc043e24 OP_EQUALVERIFY OP_CHECKSIG",
            "hex": "76a91413bbd8bc8df03a5a9cda88103f583aa8bc043e2488ac",
            "reqSigs": 1,
            "type": "pubkeyhash",
            "addresses": [
                "t1KfwsnwJeNRVjQGBDZhwKskpQbih2qx5Ua"
            ]
        },
        "version": 6,
        "coinbase": false
    },
    "id": "getblock.io"
}
```

## Response Parameters

| Parameter                | Type            | Description                                                                |
| ------------------------ | --------------- | -------------------------------------------------------------------------- |
| `bestblock`              | string          | Hash of the chain tip at query time                                        |
| `confirmations`          | integer         | Number of confirmations (0 for mempool UTXOs)                              |
| `value`                  | number          | Output value in ZEC                                                        |
| `scriptPubKey.asm`       | string          | Human-readable script disassembly                                          |
| `scriptPubKey.hex`       | string          | Hex-encoded raw script                                                     |
| `scriptPubKey.reqSigs`   | integer         | Required signature count for standard scripts                              |
| `scriptPubKey.type`      | string          | Script type (`pubkeyhash`, `scripthash`, `multisig`, `pubkey`, `nulldata`) |
| `scriptPubKey.addresses` | array of string | Transparent addresses that can spend this output                           |
| `coinbase`               | boolean         | Whether this output is from a coinbase (miner reward) transaction          |

## Use Cases

* **Wallet UTXO Verification**: Confirm a UTXO is still unspent before using it as an input
* **Explorer UTXO Detail**: Show output-level detail on transaction detail pages
* **Balance Reconstruction**: Iterate known UTXOs to reconstruct a wallet's transparent balance
* **Coinbase Maturity Checks**: Detect coinbase UTXOs and enforce the 100-confirmation maturity rule before spending

## Error Handling

| Error Code | Message        | Description                       |
| ---------- | -------------- | --------------------------------- |
| -32602     | Invalid params | TxID or output index is malformed |
| -32603     | Internal error | Node failed to look up the UTXO   |
