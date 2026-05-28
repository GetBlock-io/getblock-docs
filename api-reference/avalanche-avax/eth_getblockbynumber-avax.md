---
description: >-
  Example code for the eth_getBlockByNumber JSON-RPC method. Complete guide on
  how to use eth_getBlockByNumber JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getBlockByNumber - AVAX

Returns information about a C-Chain block by block number. Pass `"latest"`, `"earliest"`, `"pending"`, `"safe"`, or `"finalized"` as a block tag, or a hex-encoded block number. On Avalanche, sub-second finality means `"safe"` and `"finalized"` track very close to `"latest"`.

## Parameters

| Parameter        | Type    | Required | Description                                                                            |
| ---------------- | ------- | -------- | -------------------------------------------------------------------------------------- |
| blockParameter   | string  | Yes      | Block number in hex, or `"latest"`, `"earliest"`, `"pending"`, `"safe"`, `"finalized"` |
| fullTransactions | boolean | Yes      | If `true`, returns full transaction objects; if `false`, only hashes                   |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBlockByNumber",
    "params": [
        "latest",
        false
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
    "method": "eth_getBlockByNumber",
    "params": [
        "latest",
        false
    ],
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc',
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

url = "https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_getBlockByNumber",
    "params": [
        "latest",
        false
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
        "method": "eth_getBlockByNumber",
        "params": [
                "latest",
                false
        ],
        "id": "getblock.io"
});

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc")
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
        "number": "0x44b8a3c",
        "hash": "0x4f3a1d6e8c2b9a7e5d3f1c8a4b6e9d2f5a8c3e7b1d4f9a6c2e5b8d3f7a1c4e9b",
        "parentHash": "0x3a2e9d4b1c5f8a7e6d4b2c1f9a3e5d8b7c2f4a1d6e9b3c5f8a2d4e7b1c9f3a6e",
        "nonce": "0x0000000000000000",
        "logsBloom": "0x0000000000000000000000000000000000000000000000000000000000000000",
        "transactionsRoot": "0x4f3a1d6e8c2b9a7e5d3f1c8a4b6e9d2f5a8c3e7b1d4f9a6c2e5b8d3f7a1c4e9b",
        "stateRoot": "0x9e7c5e3e3a3b8e1aa0e2d4c7f9d4b0c8b8d5f1a2b3c4d5e6f7a8b9c0d1e2f3a4",
        "receiptsRoot": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
        "miner": "0x0100000000000000000000000000000000000000",
        "difficulty": "0x1",
        "totalDifficulty": "0x44b8a3c",
        "extraData": "0x",
        "size": "0x4d2",
        "gasLimit": "0xe4e1c0",
        "gasUsed": "0x2faf080",
        "baseFeePerGas": "0x5d21dba00",
        "timestamp": "0x67891f24",
        "transactions": [
            "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"
        ],
        "uncles": []
    }
}
```
{% endcode %}

## Response Parameters

| Field                | Type   | Description                                                                |
| -------------------- | ------ | -------------------------------------------------------------------------- |
| result.number        | string | Block number (hex)                                                         |
| result.hash          | string | Block hash (32 bytes hex)                                                  |
| result.parentHash    | string | Hash of the parent block                                                   |
| result.timestamp     | string | Unix timestamp at which the block was sealed (hex)                         |
| result.gasLimit      | string | Maximum gas allowed in this block (hex)                                    |
| result.gasUsed       | string | Total gas used by all transactions (hex)                                   |
| result.baseFeePerGas | string | EIP-1559 base fee per gas (hex)                                            |
| result.transactions  | array  | Array of transaction hashes, or full tx objects if `fullTransactions=true` |

## Use Cases

* Block-by-block indexing of C-Chain transactions
* Building Snowtrace-style explorers and analytics
* Computing block-level statistics (gas usage, tx count, fees)

## Error Handling

| Status Code | Error Message     | Cause                                       |
| ----------- | ----------------- | ------------------------------------------- |
| 404         | Not found         | Missing or invalid `<ACCESS-TOKEN>`         |
| -32602      | Invalid params    | Request parameters are missing or malformed |
| 429         | Too Many Requests | Rate limit exceeded for your plan           |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc');

// Most methods are exposed directly on the provider.
// For raw access, use provider.send(method, params).
const result = await provider.send('eth_getBlockByNumber', ["latest", false]);
console.log(result);
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { avalanche } from 'viem/chains';

const client = createPublicClient({
    chain: avalanche,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc')
});

const result = await client.request({ method: 'eth_getBlockByNumber', params: ["latest", false] });
console.log(result);
```
{% endtab %}
{% endtabs %}
