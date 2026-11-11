---
description: >-
  Example code for the getaddressutxos JSON-RPC method. Complete guide on how to
  use the getaddressutxos JSON-RPC method in GetBlock.io Web3 documentation.
---

# getaddressutxos - Zcash

This method returns all unspent transaction outputs (UTXOs) held by one or more transparent addresses. Optionally includes chain-info metadata (chain tip height and hash) alongside the UTXOs for consistency guarantees. **Transparent UTXOs only** — shielded notes are not enumerable.

## Parameters

| Parameter             | Type   | Required | Description                |
| --------------------- | ------ | -------- | -------------------------- |
| `addresses_or_object` | object | Yes      | Request object (see below) |

### Request Object

| Field       | Type            | Required | Description                                                             |
| ----------- | --------------- | -------- | ----------------------------------------------------------------------- |
| `addresses` | array of string | Yes      | Transparent addresses to query                                          |
| `chainInfo` | boolean         | No       | If `true`, wrap the UTXO list in a chain-info envelope. Default `false` |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getaddressutxos",
    "params": [
        {
            "addresses": [
                "t1KfwsnwJeNRVjQGBDZhwKskpQbih2qx5Ua"
            ],
            "chainInfo": true
        }
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
    method: 'getaddressutxos',
    params: [
        {
            "addresses": [
                "t1KfwsnwJeNRVjQGBDZhwKskpQbih2qx5Ua"
            ],
            "chainInfo": true
        }
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
        'method': 'getaddressutxos',
        'params': [
        {
            "addresses": [
                "t1KfwsnwJeNRVjQGBDZhwKskpQbih2qx5Ua"
            ],
            "chainInfo": true
        }
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
            "method": "getaddressutxos",
            "params": [
        {
            "addresses": [
                "t1KfwsnwJeNRVjQGBDZhwKskpQbih2qx5Ua"
            ],
            "chainInfo": true
        }
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
        "utxos": [
            {
                "address": "t1KfwsnwJeNRVjQGBDZhwKskpQbih2qx5Ua",
                "txid": "65951708c3d0087631c663179faf2b84e11d41b95fd56eae9dc4810e565415ee",
                "outputIndex": 1,
                "script": "76a91413bbd8bc8df03a5a9cda88103f583aa8bc043e2488ac",
                "satoshis": 317223382,
                "height": 3470680
            },
            {
                "address": "t1KfwsnwJeNRVjQGBDZhwKskpQbih2qx5Ua",
                "txid": "7c305b591fa0ffd3d3a4f2a27024d94d888c708a5a02f8006d378459c7ed706a",
                "outputIndex": 0,
                "script": "76a91413bbd8bc8df03a5a9cda88103f583aa8bc043e2488ac",
                "satoshis": 569684617,
                "height": 3470680
            }
        ],
        "hash": "000000000044b7b0276f3baa73a690467eaff42379eaeeb197a983c6f1408ce1",
        "height": 3487829
    },
    "id": "getblock.io"
}
```

## Response Parameters

| Parameter             | Type            | Description                                             |
| --------------------- | --------------- | ------------------------------------------------------- |
| `utxos`               | array of object | List of unspent outputs (present when `chainInfo=true`) |
| `utxos[].address`     | string          | Address holding this UTXO                               |
| `utxos[].txid`        | string          | Transaction ID containing the UTXO                      |
| `utxos[].outputIndex` | integer         | Output index within the transaction                     |
| `utxos[].script`      | string          | Hex-encoded scriptPubKey                                |
| `utxos[].satoshis`    | integer         | UTXO value in zatoshi                                   |
| `utxos[].height`      | integer         | Block height at which the UTXO was created              |
| `hash`                | string          | Chain tip hash at query time (chainInfo=true only)      |
| `height`              | integer         | Chain tip height at query time (chainInfo=true only)    |

## Use Cases

* **Wallet Coin Selection**: Enumerate UTXOs for coin-selection algorithms when constructing new transactions
* **Transparent Balance Verification**: Independently sum UTXOs to verify `getaddressbalance` results
* **Consistent Snapshot Reads**: Use `chainInfo=true` to get UTXOs alongside the tip they were read at
* **Custody Reconciliation**: Enumerate all UTXOs across a family of custody addresses for accounting

## Error Handling

| Error Code | Message         | Description                         |
| ---------- | --------------- | ----------------------------------- |
| -8         | Invalid address | One or more addresses are malformed |
| -32602     | Invalid params  | Request structure is malformed      |
| -32603     | Internal error  | Address indexing may be disabled    |
