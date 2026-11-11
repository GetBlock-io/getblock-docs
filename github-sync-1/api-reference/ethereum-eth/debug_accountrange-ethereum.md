---
description: >-
  Example code for the debug_accountRange JSON RPC method. Сomplete guide on how
  to use debug_accountRange JSON RPC in GetBlock Web3 documentation.
---

# debug\_accountRange - Ethereum

This method returns a range of accounts from the state trie at a specific block, starting from a given key prefix. Used by state-inspection tools and archival analytics to enumerate the account universe at a historical block. Typically requires archive-node access (Dedicated Node tier on GetBlock).

## Parameters

| Parameter        | Type    | Required | Description                                                                                 |
| ---------------- | ------- | -------- | ------------------------------------------------------------------------------------------- |
| `blockParameter` | string  | Yes      | Block number in hex or tag (`latest`, `finalized`, `safe`)                                  |
| `txIndex`        | integer | Yes      | Transaction index within the block to snapshot state at                                     |
| `startKey`       | string  | Yes      | Hex-encoded keccak256 hash of the starting account address (empty string for the beginning) |
| `maxResults`     | integer | Yes      | Maximum number of accounts to return in this page                                           |
| `nocode`         | boolean | Yes      | If `true`, omit contract bytecode from results                                              |
| `nostorage`      | boolean | Yes      | If `true`, omit storage root from results                                                   |
| `incompletes`    | boolean | Yes      | If `true`, include accounts with incomplete state                                           |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "debug_accountRange",
    "params": [
        "latest",
        0,
        "",
        5,
        true,
        true,
        false
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
    method: 'debug_accountRange',
    params: [
        "latest",
        0,
        "",
        5,
        true,
        true,
        false
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
        'method': 'debug_accountRange',
        'params': [
        "latest",
        0,
        "",
        5,
        true,
        true,
        false
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
            "method": "debug_accountRange",
            "params": [
        "latest",
        0,
        "",
        5,
        true,
        true,
        false
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
        "accounts": {
            "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045": {
                "balance": "0x2c68af0bb140000",
                "nonce": 1122,
                "root": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
                "codeHash": "0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470"
            }
        },
        "next": "0x000f43f9b2e2c7ba7c2b6e9c8f3c6d1e2f4a5b6c7d8e9f0a1b2c3d4e5f6a7b8c9"
    }
}
```
{% endcode %}

## Response Parameters

| Parameter         | Type   | Description                                                                      |
| ----------------- | ------ | -------------------------------------------------------------------------------- |
| `jsonrpc`         | string | JSON-RPC protocol version ("2.0")                                                |
| `id`              | string | Request identifier matching the request                                          |
| `result.accounts` | object | Map of address → account object (balance, nonce, storage root, code hash)        |
| `result.next`     | string | Continuation cursor for the next page (empty string when the range is exhausted) |

## Use Cases

* **State Enumeration**: Enumerate all accounts at a historical block for airdrops or forensic analysis
* **Archive Analytics**: Feed account-universe data into research pipelines analyzing state distribution
* **Whale Discovery**: Iterate through accounts filtered by balance thresholds for wealth-distribution studies

## Error Handling

| Error Code | Message          | Description                                                                                    |
| ---------- | ---------------- | ---------------------------------------------------------------------------------------------- |
| -32601     | Method not found | `debug` module not enabled on this endpoint — Dedicated Node tier required                     |
| -32602     | Invalid params   | Block parameter, start key, or maxResults is malformed                                         |
| -32603     | Internal error   | Node failed to enumerate the state trie (often indicates state pruning at the requested block) |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const range = await provider.send('debug_accountRange', ['latest', 0, '', 5, true, true, false]);
console.log('Accounts:', Object.keys(range.accounts).length);
console.log('Next cursor:', range.next);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { mainnet } from 'viem/chains';

const client = createPublicClient({
    chain: mainnet,
    transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/'),
});

const range = await client.request({
    method: 'debug_accountRange',
    params: ['latest', 0, '', 5, true, true, false],
});
console.log('Accounts:', Object.keys(range.accounts).length);
```
{% endcode %}
{% endtab %}
{% endtabs %}
