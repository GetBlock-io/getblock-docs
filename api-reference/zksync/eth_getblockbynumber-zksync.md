---
description: >-
  Example code for the eth_getBlockByNumber JSON-RPC method. Сomplete guide on
  how to use eth_getBlockByNumber JSON-RPC in GetBlock.io Web3 documentation.
---

# eth\_getBlockByNumber - zkSync

Returns information about an L2 block by block number. Pass `"latest"`, `"earliest"`, `"pending"`, `"safe"`, or `"finalized"` as a block tag, or a hex-encoded block number. `"finalized"` reflects the latest block whose L1 batch has been executed on the Ethereum mainnet.

## Parameters

| Parameter        | Type    | Required | Description                                                                            |
| ---------------- | ------- | -------- | -------------------------------------------------------------------------------------- |
| blockParameter   | string  | Yes      | Block number in hex, or `"latest"`, `"earliest"`, `"pending"`, `"safe"`, `"finalized"` |
| fullTransactions | boolean | Yes      | If `true`, returns full transaction objects; if `false`, only hashes                   |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
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
        "number": "0x2249b7",
        "hash": "0x4f3a1d6e8c2b9a7e5d3f1c8a4b6e9d2f5a8c3e7b1d4f9a6c2e5b8d3f7a1c4e9b",
        "parentHash": "0x3a2e9d4b1c5f8a7e6d4b2c1f9a3e5d8b7c2f4a1d6e9b3c5f8a2d4e7b1c9f3a6e",
        "nonce": "0x0000000000000000",
        "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
        "logsBloom": "0x0000000000000000000000000000000000000000000000000000000000000000",
        "transactionsRoot": "0x4f3a1d6e8c2b9a7e5d3f1c8a4b6e9d2f5a8c3e7b1d4f9a6c2e5b8d3f7a1c4e9b",
        "stateRoot": "0xf1adac176fc939313eea4b72055db0622a10bbd9b7a83097286e84e471d2e7df",
        "receiptsRoot": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
        "miner": "0xfeee860e7aae671124e9a4e61139f3a5085dfeee",
        "difficulty": "0x0",
        "totalDifficulty": "0x0",
        "extraData": "0x",
        "size": "0x4d2",
        "gasLimit": "0xffffffff",
        "gasUsed": "0x2faf080",
        "timestamp": "0x67891f24",
        "transactions": [
            "0x22de7debaa98758afdaee89f447ff43bab5da3de6acca7528b281cc2f1be2ee9"
        ],
        "uncles": [],
        "l1BatchNumber": "0x72582",
        "l1BatchTimestamp": "0x67891f00"
    }
}
```
{% endcode %}

## Response Parameters

| Field                   | Type   | Description                                                                         |
| ----------------------- | ------ | ----------------------------------------------------------------------------------- |
| result.number           | string | L2 block number (hex)                                                               |
| result.hash             | string | L2 block hash (32 bytes hex)                                                        |
| result.parentHash       | string | Hash of the parent L2 block                                                         |
| result.timestamp        | string | Unix timestamp of the block (hex)                                                   |
| result.gasUsed          | string | Total gas used by transactions in this block (hex)                                  |
| result.transactions     | array  | Array of transaction hashes, or full transaction objects if `fullTransactions=true` |
| result.l1BatchNumber    | string | L1 batch number containing this L2 block (zkSync extension)                         |
| result.l1BatchTimestamp | string | Unix timestamp of the L1 batch (zkSync extension)                                   |

## Use Cases

* Block-by-block indexing of zkSync Era transactions
* Building explorers showing L1 batch grouping
* Computing per-block statistics on L2

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
const result = await provider.send('eth_getBlockByNumber', ["latest", false]);
console.log(result);
```
{% endtab %}

{% tab title="zksync2-python (Python)" %}
```python
from zksync2.module.module_builder import ZkSyncBuilder

zk_web3 = ZkSyncBuilder.build('https://go.getblock.io/<ACCESS-TOKEN>/')

# zksync2-python exposes the JSON-RPC layer directly.
result = zk_web3.zksync._zks_endpoints if 'eth_getBlockByNumber'.startswith('zks_') else None
# Or call any method generically:
result = zk_web3.provider.make_request('eth_getBlockByNumber', ["latest", false])
print(result)
```
{% endtab %}
{% endtabs %}
