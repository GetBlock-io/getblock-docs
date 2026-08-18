---
description: >-
  Example code for the eth_createAccessList JSON RPC method. Сomplete guide on
  how to use eth_createAccessList JSON RPC in GetBlock Web3 documentation.
---

# eth\_createAccessList - Ethereum

This method returns an EIP-2930 access list for a transaction — the list of accounts and storage slots the transaction would touch during execution. Transactions with a pre-declared access list pay lower gas per SLOAD/BALANCE than without one (though the access list itself has a size cost). Nodes and MEV searchers use this to pre-compute optimal access lists.

## Parameters

| Parameter        | Type   | Required | Description                                                                                        |
| ---------------- | ------ | -------- | -------------------------------------------------------------------------------------------------- |
| `transaction`    | object | Yes      | Transaction call object — same shape as `eth_call` transaction object                              |
| `blockParameter` | string | No       | Block number in hex, or `latest`, `earliest`, `pending`, `finalized`, `safe`. Defaults to `latest` |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_createAccessList",
    "params": [
        {
            "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
            "to": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
            "data": "0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045"
        },
        "latest"
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
    method: 'eth_createAccessList',
    params: [
        {
            "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
            "to": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
            "data": "0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045"
        },
        "latest"
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
        'method': 'eth_createAccessList',
        'params': [
        {
            "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
            "to": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
            "data": "0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045"
        },
        "latest"
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
            "method": "eth_createAccessList",
            "params": [
        {
            "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
            "to": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
            "data": "0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045"
        },
        "latest"
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
        "accessList": [
            {
                "address": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
                "storageKeys": [
                    "0x9b30e0da3d7a1c88b4bcc11f6efe72fcabbb64b1ac0f1a2d4de1abf0feaa1234"
                ]
            }
        ],
        "gasUsed": "0x8ba0"
    }
}
```
{% endcode %}

## Response Parameters

| Parameter           | Type            | Description                                                          |
| ------------------- | --------------- | -------------------------------------------------------------------- |
| `jsonrpc`           | string          | JSON-RPC protocol version ("2.0")                                    |
| `id`                | string          | Request identifier matching the request                              |
| `result.accessList` | array of object | The computed access list — array of `{address, storageKeys}` entries |
| `result.gasUsed`    | string          | Total gas the transaction would consume with this access list        |

## Use Cases

* **Type-1 Transaction Optimization**: Wrap a transaction in a type-1 EIP-2930 transaction with the returned access list for slightly lower gas cost
* **MEV Bundle Optimization**: Precompute access lists for bundle transactions to reduce total bundle gas
* **Complex Call Preflight**: Understand what state a complex call will touch before executing it on-chain
* **Multi-Contract Access Analysis**: Reveal cross-contract state touches for security analysis or gas budgeting

## Error Handling

| Error Code | Message            | Description                                    |
| ---------- | ------------------ | ---------------------------------------------- |
| -32602     | Invalid params     | Transaction object is malformed                |
| 3          | Execution reverted | The transaction would revert during simulation |
| -32000     | Execution error    | Node failed to compute the access list         |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const result = await provider.send('eth_createAccessList', [
    { from: '0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045', to: '0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2', data: '0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045' },
    'latest'
]);
console.log('Access list:', result.accessList);
console.log('Gas used with access list:', result.gasUsed);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" overflow="wrap" %}
```javascript
import { createPublicClient, http } from 'viem';
import { mainnet } from 'viem/chains';

const client = createPublicClient({
    chain: mainnet,
    transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/'),
});

const result = await client.createAccessList({
    account: '0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045',
    to: '0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2',
    data: '0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045',
});
console.log('Access list:', result.accessList);
console.log('Gas used:', result.gasUsed);
```
{% endcode %}
{% endtab %}
{% endtabs %}
