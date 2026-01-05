---
description: >-
  Example code for the eth_getBlockByNumber JSON-RPC method. Complete guide on
  how to use eth_getBlockByNumber JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getBlockByNumber - Mantle

This method returns information about a block on Mantle by block number.

### Parameters

| Parameter        | Type    | Description                                                                     |
| ---------------- | ------- | ------------------------------------------------------------------------------- |
| blockNumber      | string  | Block number in hex, or "latest", "earliest", "pending"                         |
| fullTransactions | boolean | If true, returns full transaction objects; if false, returns transaction hashes |

### Request

{% tabs %}
{% tab title="curl" %}
{% code title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBlockByNumber",
    "params": ["0x3F6777", false],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (axios)" %}
{% code title="axios example" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_getBlockByNumber",
    "params": ["0x3F6777", false],
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
    .then(response => console.log(JSON.stringify(response.data)))
    .catch(error => console.log(error));
```
{% endcode %}
{% endtab %}

{% tab title="Python (requests)" %}
{% code title="python example" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_getBlockByNumber",
    "params": ["0x3F6777", False],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endcode %}
{% endtab %}

{% tab title="Rust (reqwest)" %}
{% code title="rust example" %}
```rust
use reqwest::header;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = reqwest::Client::new();
    
    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header(header::CONTENT_TYPE, "application/json")
        .body(r#"{
            "jsonrpc": "2.0",
            "method": "eth_getBlockByNumber",
            "params": ["0x3F6777", false],
            "id": "getblock.io"
        }"#)
        .send()
        .await?;
    
    println!("{}", response.text().await?);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Response

{% code title="Response (JSON)" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "difficulty": "0x2",
        "extraData": "0xd98301090a846765746889676f312e31352e3133856c696e7578",
        "gasLimit": "0xe4e1c0",
        "gasUsed": "0x5208",
        "hash": "0x9b14d73f45c836bfb0e1f59453c39fe8cddab45a0f78670dc6190e5c85b65a8e",
        "logsBloom": "0x00000000...",
        "miner": "0x0000000000000000000000000000000000000000",
        "mixHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
        "nonce": "0x0000000000000000",
        "number": "0x3f6777",
        "parentHash": "0xd9505c7d82fbbad090d3ab4b4ecd20bca0f3700ce0c0c5ad2563a4100f86a004",
        "receiptsRoot": "0x056b23fbba480696b65fe5a59b8f2148a1299103c4f57df839233af2cf4ca2d2",
        "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
        "size": "0x2d1",
        "stateRoot": "0x6ac1007f49aecaa29deff133703b287eb166ed12826bb685520f4b7a1aace96a",
        "timestamp": "0x643fc8a5",
        "totalDifficulty": "0x7eceef",
        "transactions": [
            "0x6dfef681e0a83a40b05d40877e53a88459e8829240a6d6d5ec6fe816e435f8d8"
        ],
        "transactionsRoot": "0x...",
        "uncles": []
    }
}
```
{% endcode %}

### Response Parameters

| Field        | Type   | Description                            |
| ------------ | ------ | -------------------------------------- |
| number       | string | Block number (hex)                     |
| hash         | string | Block hash (32 bytes)                  |
| parentHash   | string | Parent block hash                      |
| timestamp    | string | Unix timestamp of block creation       |
| gasLimit     | string | Maximum gas allowed in block           |
| gasUsed      | string | Total gas used by transactions         |
| transactions | array  | Array of transaction hashes or objects |
| miner        | string | Address of block producer              |

### Use Case

The `eth_getBlockByNumber` method is essential for:

* Block explorer functionality
* Transaction confirmation tracking
* Historical data analysis
* Chain synchronization verification
* DeFi protocol state tracking
* Event indexing by block range

### Error Handling

| Status Code | Error Message  | Cause                           |
| ----------- | -------------- | ------------------------------- |
| 403         | Forbidden      | Missing or invalid ACCESS-TOKEN |
| -32602      | Invalid params | Invalid block number format     |

### Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js example" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const block = await provider.getBlock(4155255);
console.log('Block:', block);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem example" %}
```javascript
import { createPublicClient, http } from 'viem';
import { mantle } from 'viem/chains';

const client = createPublicClient({
    chain: mantle,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const block = await client.getBlock({
    blockNumber: 4155255n
});
console.log('Block:', block);
```
{% endcode %}
{% endtab %}
{% endtabs %}
