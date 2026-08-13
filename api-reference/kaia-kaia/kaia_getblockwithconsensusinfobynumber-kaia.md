---
description: >-
  Example code for the kaia_getBlockWithConsensusInfoByNumber JSON RPC method.
  Complete guide on how to use kaia_getBlockWithConsensusInfoByNumber JSON RPC
  in GetBlock Web3 documentation.
---

# kaia\_getblockwithconsensusinfobynumber

This method returns a block along with its consensus information: the proposer that produced the block and the committee of validators that signed it. This exposes Kaia's BFT consensus for a given block.

## Parameters

| Parameter | Type   | Required | Description                      |
| --------- | ------ | -------- | -------------------------------- |
| block     | string | Yes      | Block number in hex, or "latest" |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "kaia_getBlockWithConsensusInfoByNumber",
    "params": ["0x1b4"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'kaia_getBlockWithConsensusInfoByNumber',
    params: ["0x1b4"],
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
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'kaia_getBlockWithConsensusInfoByNumber',
        'params': ["0x1b4"],
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
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "kaia_getBlockWithConsensusInfoByNumber",
            "params": ["0x1b4"],
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
        "blockscore": "0x1",
        "committee": [
            "0x571e53df607be97431a5bbefca1dffe5aef56f4d",
            "0x5cb1a7dccbd0dc446e3640898ede8820368554c8",
            "0x99fb17d324fa0e07f23b49d09028ac0919414db6"
        ],
        "hash": "0x7f5f...c3a1",
        "number": "0x1b4",
        "proposer": "0x571e53df607be97431a5bbefca1dffe5aef56f4d",
        "originProposer": "0x571e53df607be97431a5bbefca1dffe5aef56f4d",
        "round": 0,
        "timestamp": "0x66a1b2c0",
        "transactions": []
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                       |
| --------- | ------ | ------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                 |
| id        | string | Request identifier matching the request           |
| result    | object | Block object augmented with consensus information |

### Result Object

| Field          | Type    | Description                                                 |
| -------------- | ------- | ----------------------------------------------------------- |
| proposer       | string  | Address of the validator that proposed the block            |
| committee      | array   | Validator addresses that formed the block's committee       |
| originProposer | string  | Proposer at round 0, if the block went through extra rounds |
| round          | integer | Consensus round the block was finalized in                  |
| transactions   | array   | Transactions included in the block                          |

## Use Cases

* **Consensus Analytics**: Read the proposer and committee for a block
* **Validator Monitoring**: Track which validators sign blocks
* **Governance Tooling**: Audit block production across the council
* **Explorer Detail**: Show consensus metadata on a block page

## Error Handling

| Error Code | Message        | Description                                                |
| ---------- | -------------- | ---------------------------------------------------------- |
| -32602     | Invalid params | A parameter is missing or has the wrong type or format     |
| -32000     | Not found      | The account or block does not exist in the requested state |
| -32603     | Internal error | The node failed to process the request                     |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const block = await provider.send('kaia_getBlockWithConsensusInfoByNumber', ['0x1b4']);
console.log(block.proposer, block.committee);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { kaia } from 'viem/chains';

const client = createPublicClient({ chain: kaia, transport: http('https://go.getblock.io/<ACCESS-TOKEN>/') });

const block = await client.request({
    method: 'kaia_getBlockWithConsensusInfoByNumber',
    params: ['0x1b4']
});
console.log(block.proposer, block.committee);
```
{% endcode %}
{% endtab %}
{% endtabs %}
