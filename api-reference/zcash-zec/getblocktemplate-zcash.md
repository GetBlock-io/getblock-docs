---
description: >-
  Example code for the getblocktemplate JSON-RPC method. Сomplete guide on how
  to use the getblocktemplate JSON-RPC method in GetBlock.io Web3 documentation.
---

# getblocktemplate - Zcash

This method returns a template for constructing a candidate block for mining. Miners and mining pools call this to receive the current block header parameters, coinbase output structure, and the set of mempool transactions that should be included. Zcash uses the Equihash proof-of-work algorithm, so template consumers need to solve the Equihash puzzle for the provided parameters.

## Parameters

| Parameter          | Type   | Required | Description                                                                        |
| ------------------ | ------ | -------- | ---------------------------------------------------------------------------------- |
| `template_request` | object | No       | Template request parameters (see below). Defaults to the standard template request |

### Template Request

| Field          | Type            | Required | Description                                                                       |
| -------------- | --------------- | -------- | --------------------------------------------------------------------------------- |
| `mode`         | string          | No       | `template` (default) requests a new template; `proposal` submits a proposed block |
| `capabilities` | array of string | No       | Client capabilities (`coinbasetxn`, `coinbasevalue`, `proposal`)                  |
| `longpollid`   | string          | No       | Long-polling ID for waiting on updated templates                                  |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getblocktemplate",
    "params": [
        {
            "mode": "template",
            "capabilities": [
                "coinbasetxn",
                "coinbasevalue"
            ]
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
    method: 'getblocktemplate',
    params: [
        {
            "mode": "template",
            "capabilities": [
                "coinbasetxn",
                "coinbasevalue"
            ]
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
        'method': 'getblocktemplate',
        'params': [
        {
            "mode": "template",
            "capabilities": [
                "coinbasetxn",
                "coinbasevalue"
            ]
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
            "method": "getblocktemplate",
            "params": [
        {
            "mode": "template",
            "capabilities": [
                "coinbasetxn",
                "coinbasevalue"
            ]
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
        "capabilities": [
            "proposal"
        ],
        "version": 4,
        "previousblockhash": "00000000018bb43cadf5b4c8a2e7f9d0e1a6b3c8d5e4f2a1b0c9d8e7f6a5b4c3",
        "blockcommitmentshash": "5eae9c5cbe0e8d3e2f5cd8b0a4e3b1f2c9d8e7f6a5b4c3d2e1f0a9b8c7d6e5f4",
        "lightclientroothash": "3a2b1c9d8e7f6a5b4c3d2e1f0a9b8c7d6e5f4a3b2c1d0e9f8a7b6c5d4e3f2a1b",
        "finalsaplingroothash": "01a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8a9b0c1d2e3f4a5b6c7d8e9f0a1",
        "defaultroots": {
            "merkleroot": "9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b3a2b1c9d8e7f6a5b4c3d2e1f",
            "chainhistoryroot": "b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8a9b0c1d2e3f4a5b6c7d8e9f0a1b2c3",
            "authdataroot": "9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b3a2b1c9d8e7f6a5b4c3d2e1f"
        },
        "transactions": [],
        "coinbasetxn": {
            "data": "0400008085202f890100000000000000000000000000000000...",
            "hash": "2b1c9d8e7f6a5b4c3d2e1f0a9b8c7d6e5f4a3b2c1d0e9f8a7b6c5d4e3f2a1b09",
            "authdigest": "0000000000000000000000000000000000000000000000000000000000000000",
            "depends": [],
            "fee": 0,
            "sigops": 0,
            "required": true
        },
        "longpollid": "00000000018bb43cadf5b4c8a2e7f9d0e1a6b3c8d5e4f2a1b0c9d8e7f6a5b4c3-47",
        "target": "00000000005cea00000000000000000000000000000000000000000000000000",
        "mintime": 1747852500,
        "mutable": [
            "time",
            "transactions",
            "prevblock"
        ],
        "noncerange": "00000000ffffffff",
        "sigoplimit": 20000,
        "sizelimit": 2000000,
        "curtime": 1747852800,
        "bits": "1d00ffff",
        "height": 2856343
    }
}
```

## Response Parameters

| Parameter              | Type            | Description                                             |
| ---------------------- | --------------- | ------------------------------------------------------- |
| `version`              | integer         | Block version to use                                    |
| `previousblockhash`    | string          | Hash of the parent block                                |
| `blockcommitmentshash` | string          | Block commitments hash to include in the header         |
| `finalsaplingroothash` | string          | Final Sapling root at this block height                 |
| `defaultroots`         | object          | Precomputed default root hashes for header construction |
| `transactions`         | array of object | Mempool transactions selected for inclusion             |
| `coinbasetxn`          | object          | Coinbase transaction template with reward split         |
| `target`               | string          | Difficulty target to solve for                          |
| `mintime`              | integer         | Minimum allowed block timestamp                         |
| `mutable`              | array of string | Fields the miner may modify                             |
| `noncerange`           | string          | Nonce range hint                                        |
| `sigoplimit`           | integer         | Maximum signature operations allowed in the block       |
| `sizelimit`            | integer         | Maximum block size in bytes                             |
| `curtime`              | integer         | Current timestamp suggested by the node                 |
| `bits`                 | string          | Compact difficulty target                               |
| `height`               | integer         | Height at which this block will be included             |

## Use Cases

* **Solo Mining**: Miners request templates to construct candidate blocks for Equihash solving
* **Mining Pool Coordination**: Pool servers fetch templates and distribute solving work to participant miners
* **Block Proposal Verification**: Submit constructed blocks in `proposal` mode to check validity before broadcasting
* **Long-Poll Template Updates**: Use `longpollid` to wait for new templates when the chain tip advances

## Error Handling

| Error Code | Message           | Description                               |
| ---------- | ----------------- | ----------------------------------------- |
| -8         | Invalid parameter | Template request parameters are malformed |
| -32602     | Invalid params    | Template request structure is invalid     |
| -32603     | Internal error    | Node failed to build the template         |
