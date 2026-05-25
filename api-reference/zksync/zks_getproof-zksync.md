---
description: >-
  Example code for the zks_getProof JSON-RPC method. Сomplete guide on how to
  use zks_getProof JSON-RPC in GetBlock.io Web3 documentation.
---

# zks\_getProof - zkSync

Generates Merkle proofs for one or more storage values associated with an account. Unlike Ethereum's two-level hexadecimal trie, zkSync Era uses a single-level full binary tree with 256-bit keys — proofs are batch-anchored rather than block-anchored, so the third parameter is an L1 batch number.

## Parameters

| Parameter     | Type            | Required | Description                                                    |
| ------------- | --------------- | -------- | -------------------------------------------------------------- |
| address       | string          | Yes      | 20-byte account address to fetch storage values and proofs for |
| storageKeys   | array of string | Yes      | Array of 32-byte storage keys to prove                         |
| l1BatchNumber | integer         | Yes      | L1 batch number at which the proof is anchored                 |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "zks_getProof",
    "params": [
        "0x0000000000000000000000000000000000008003",
        [
            "0x8b65c0cf1012ea9f393197eb24619fd814379b298b238285649e14f936a5eb12"
        ],
        354895
    ],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "zks_getProof",
    "params": [
        "0x0000000000000000000000000000000000008003",
        [
            "0x8b65c0cf1012ea9f393197eb24619fd814379b298b238285649e14f936a5eb12"
        ],
        354895
    ],
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
    "method": "zks_getProof",
    "params": [
        "0x0000000000000000000000000000000000008003",
        [
            "0x8b65c0cf1012ea9f393197eb24619fd814379b298b238285649e14f936a5eb12"
        ],
        354895
    ],
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
        "method": "zks_getProof",
        "params": [
                "0x0000000000000000000000000000000000008003",
                [
                        "0x8b65c0cf1012ea9f393197eb24619fd814379b298b238285649e14f936a5eb12"
                ],
                354895
        ],
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
        "address": "0x0000000000000000000000000000000000008003",
        "storageProof": [
            {
                "key": "0x8b65c0cf1012ea9f393197eb24619fd814379b298b238285649e14f936a5eb12",
                "proof": [
                    "0xe3e8e49a998b3abf8926f62a5a832d829aadc1b7e059f1ea59ffbab8e11edfb7"
                ],
                "value": "0x0000000000000000000000000000000000000000000000000000000000000060",
                "index": 27900957
            }
        ]
    }
}
```
{% endcode %}

## Response Parameters

| Field                        | Type            | Description                                                          |
| ---------------------------- | --------------- | -------------------------------------------------------------------- |
| result.address               | string          | Account address (echoed)                                             |
| result.storageProof          | array           | Array of storage proofs, one per requested key                       |
| result.storageProof\[].key   | string          | Storage key (echoed)                                                 |
| result.storageProof\[].value | string          | Value at the key at the time of the specified L1 batch               |
| result.storageProof\[].index | integer         | 1-based index of the entry in the Merkle tree                        |
| result.storageProof\[].proof | array of string | Merkle path from leaf to root (hashes only; root is published on L1) |

## Use Cases

* Light client state verification against zkSync's binary tree
* Cross-chain bridges proving zkSync state
* Trust-minimized storage reads for audits and bridges

## Error Handling

| Status Code | Error Message     | Cause                                       |
| ----------- | ----------------- | ------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`         |
| -32602      | Invalid params    | Request parameters are missing or malformed |
| 429         | Too Many Requests | Rate limit exceeded for your plan           |

## SDK Integration

{% tabs %}
{% tab title="zksync-ethers (JavaScript)" %}
```javascript
import { Provider } from 'zksync-ethers';

const provider = new Provider('https://go.getblock.io/<ACCESS-TOKEN>/');

// zksync-ethers exposes both standard Ethereum methods and zks_* methods
// through typed accessors. For raw access:
const result = await provider.send('zks_getProof', ["0x0000000000000000000000000000000000008003", ["0x8b65c0cf1012ea9f393197eb24619fd814379b298b238285649e14f936a5eb12"], 354895]);
console.log(result);
```
{% endtab %}

{% tab title="zksync2-python (Python)" %}
```python
from zksync2.module.module_builder import ZkSyncBuilder

zk_web3 = ZkSyncBuilder.build('https://go.getblock.io/<ACCESS-TOKEN>/')

# zksync2-python exposes the JSON-RPC layer directly.
result = zk_web3.zksync._zks_endpoints if 'zks_getProof'.startswith('zks_') else None
# Or call any method generically:
result = zk_web3.provider.make_request('zks_getProof', ["0x0000000000000000000000000000000000008003", ["0x8b65c0cf1012ea9f393197eb24619fd814379b298b238285649e14f936a5eb12"], 354895])
print(result)
```
{% endtab %}
{% endtabs %}
