# eth\_getblockreceipts zksync

Returns all transaction receipts for an L2 block. More efficient than calling `eth_getTransactionReceipt` per transaction.

## Parameters

| Parameter      | Type   | Required | Description                                   |
| -------------- | ------ | -------- | --------------------------------------------- |
| blockParameter | string | Yes      | Block number in hex, block tag, or block hash |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBlockReceipts",
    "params": [
        "latest"
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
    "method": "eth_getBlockReceipts",
    "params": [
        "latest"
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
    "method": "eth_getBlockReceipts",
    "params": [
        "latest"
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
        "method": "eth_getBlockReceipts",
        "params": [
                "latest"
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
    "result": [
        {
            "blockHash": "0x4f3a1d6e8c2b9a7e5d3f1c8a4b6e9d2f5a8c3e7b1d4f9a6c2e5b8d3f7a1c4e9b",
            "blockNumber": "0x2249b7",
            "contractAddress": null,
            "cumulativeGasUsed": "0x12345",
            "effectiveGasPrice": "0xee6b280",
            "from": "0x36615cf349d7f6344891b1e7ca7c72883f5dc049",
            "gasUsed": "0x12345",
            "logs": [],
            "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
            "status": "0x1",
            "to": "0xd85498dbeaeb1df24be52eed4f52eac2fbd56245",
            "transactionHash": "0x22de7debaa98758afdaee89f447ff43bab5da3de6acca7528b281cc2f1be2ee9",
            "transactionIndex": "0x0",
            "type": "0x2",
            "l1BatchNumber": "0x72582",
            "l1BatchTxIndex": "0x5"
        }
    ]
}
```
{% endcode %}

## Response Parameters

| Field                    | Type   | Description                                                      |
| ------------------------ | ------ | ---------------------------------------------------------------- |
| result                   | array  | Array of receipt objects, one per transaction                    |
| result\[].status         | string | `0x1` for success, `0x0` for revert                              |
| result\[].gasUsed        | string | Gas used by this transaction (hex)                               |
| result\[].logs           | array  | Logs emitted by the transaction                                  |
| result\[].l1BatchNumber  | string | L1 batch number (zkSync extension)                               |
| result\[].l1BatchTxIndex | string | Index of this transaction within the L1 batch (zkSync extension) |

## Use Cases

* Indexing all transaction outcomes for a block in one call
* Building gas-cost analytics per block
* Detecting reverted transactions and contract deployments

## Error Handling

| Status Code | Error Message     | Cause                                                |
| ----------- | ----------------- | ---------------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                  |
| -32602      | Invalid params    | Request parameters are missing or malformed          |
| -32601      | Method not found  | Method does not exist or is not enabled on this node |
| 429         | Too Many Requests | Rate limit exceeded for your plan                    |

## SDK Integration

{% tabs %}
{% tab title="zksync-ethers (JavaScript)" %}
```javascript
import { Provider } from 'zksync-ethers';

const provider = new Provider('https://go.getblock.io/<ACCESS-TOKEN>/');

// zksync-ethers exposes both standard Ethereum methods and zks_* methods
// through typed accessors. For raw access:
const result = await provider.send('eth_getBlockReceipts', ["latest"]);
console.log(result);
```
{% endtab %}

{% tab title="zksync2-python (Python)" %}
```python
from zksync2.module.module_builder import ZkSyncBuilder

zk_web3 = ZkSyncBuilder.build('https://go.getblock.io/<ACCESS-TOKEN>/')

# zksync2-python exposes the JSON-RPC layer directly.
result = zk_web3.zksync._zks_endpoints if 'eth_getBlockReceipts'.startswith('zks_') else None
# Or call any method generically:
result = zk_web3.provider.make_request('eth_getBlockReceipts', ["latest"])
print(result)
```
{% endtab %}
{% endtabs %}
