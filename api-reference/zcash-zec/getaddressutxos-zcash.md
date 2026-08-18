---
description: >-
  Example code for the getaddressutxos JSON-RPC method. Сomplete guide on how to
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
                "t1c74RiTicVSKthLmn2c4WFLJdLDs5sQ4X6"
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
                "t1c74RiTicVSKthLmn2c4WFLJdLDs5sQ4X6"
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
                "t1c74RiTicVSKthLmn2c4WFLJdLDs5sQ4X6"
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
                "t1c74RiTicVSKthLmn2c4WFLJdLDs5sQ4X6"
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
    "id": "getblock.io",
    "result": {
        "utxos": [
            {
                "address": "t1c74RiTicVSKthLmn2c4WFLJdLDs5sQ4X6",
                "txid": "9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b3a2b1c9d8e7f6a5b4c3d2e1f",
                "outputIndex": 0,
                "script": "76a914abc...88ac",
                "satoshis": 149999000,
                "height": 2856337
            }
        ],
        "hash": "00000000018bb43cadf5b4c8a2e7f9d0e1a6b3c8d5e4f2a1b0c9d8e7f6a5b4c3",
        "height": 2856342
    }
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
