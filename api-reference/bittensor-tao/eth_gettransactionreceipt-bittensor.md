# eth\_gettransactionreceipt bittensor

{% hint style="info" %}
**Subtensor EVM JSON-RPC method.** Call against a GetBlock endpoint configured for the **EVM interface** (not the Substrate interface). Subtensor EVM uses chain ID **945** (`0x3b1`). Standard Ethereum tooling — ethers.js, viem, Hardhat, Foundry, MetaMask — works unmodified.
{% endhint %}

Returns the receipt of a confirmed EVM transaction — gas used, logs emitted, status (1 = success, 0 = revert), and contract address if the transaction created a contract.

## Parameters

| Parameter | Type            | Required | Description      |
| --------- | --------------- | -------- | ---------------- |
| txHash    | DATA (32 bytes) | Yes      | Transaction hash |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getTransactionReceipt",
    "params": [
        "0xa8f9b3c7d2e4f6a1b9c8d7e6f5a4b3c2d1e0f9a8b7c6d5e4f3a2b1c0d9e8f7a6"
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
    "method": "eth_getTransactionReceipt",
    "params": [
        "0xa8f9b3c7d2e4f6a1b9c8d7e6f5a4b3c2d1e0f9a8b7c6d5e4f3a2b1c0d9e8f7a6"
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
    "method": "eth_getTransactionReceipt",
    "params": [
        "0xa8f9b3c7d2e4f6a1b9c8d7e6f5a4b3c2d1e0f9a8b7c6d5e4f3a2b1c0d9e8f7a6"
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
        "method": "eth_getTransactionReceipt",
        "params": [
                "0xa8f9b3c7d2e4f6a1b9c8d7e6f5a4b3c2d1e0f9a8b7c6d5e4f3a2b1c0d9e8f7a6"
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
        "transactionHash": "0xa8f9b3c7d2e4f6a1b9c8d7e6f5a4b3c2d1e0f9a8b7c6d5e4f3a2b1c0d9e8f7a6",
        "blockHash": "0xc0a6cfca5d4f9e1cb8a5c4b3d2e7a9c4f1b8e6d5c3a2b1f9e8d7c6b5a4f3e2d1c",
        "blockNumber": "0x599f5a",
        "from": "0x7a3F8b9c2d1e4a6f8c5b3d2e7a9c4f1b8e6d5c3a",
        "to": "0x4a5d8E91234567890abcdef1234567890ABCDEF12",
        "gasUsed": "0x5208",
        "cumulativeGasUsed": "0x5208",
        "status": "0x1",
        "logs": [],
        "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
        "contractAddress": null,
        "effectiveGasPrice": "0x3b9aca00",
        "type": "0x2"
    }
}
```
{% endcode %}

## Response Parameters

| Field                  | Type         | Description                                  |
| ---------------------- | ------------ | -------------------------------------------- |
| result.status          | QUANTITY     | `0x1` = success, `0x0` = reverted            |
| result.gasUsed         | QUANTITY     | Actual gas consumed                          |
| result.logs            | array of Log | Event logs emitted during execution          |
| result.contractAddress | DATA         | Address of newly created contract, or `null` |

## Use Cases

* Verifying EVM transaction success or failure
* Reading event logs for state updates on Subtensor EVM contracts

## Error Handling

| Status Code | Error Message     | Cause                                                                                                                      |
| ----------- | ----------------- | -------------------------------------------------------------------------------------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                                                                                        |
| -32602      | Invalid params    | Request parameters are missing or malformed                                                                                |
| -32601      | Method not found  | Method does not exist on this interface — check whether you're calling a Substrate method on an EVM endpoint or vice versa |
| -32603      | Internal error    | Server-side error while processing the request                                                                             |
| 429         | Too Many Requests | Rate limit exceeded for your plan                                                                                          |

## SDK Integration

{% tabs %}
{% tab title="ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// Generic JSON-RPC call (works for any EVM method):
const result = await provider.send('eth_getTransactionReceipt', ["0xa8f9b3c7d2e4f6a1b9c8d7e6f5a4b3c2d1e0f9a8b7c6d5e4f3a2b1c0d9e8f7a6"]);
console.log(result);

// Most standard methods have typed wrappers:
// provider.getBalance(addr), provider.getBlock(n), provider.getCode(addr), etc.
```
{% endtab %}

{% tab title="viem" %}
```javascript
import { createPublicClient, http } from 'viem';

const client = createPublicClient({
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const result = await client.request({
    method: 'eth_getTransactionReceipt',
    params: ["0xa8f9b3c7d2e4f6a1b9c8d7e6f5a4b3c2d1e0f9a8b7c6d5e4f3a2b1c0d9e8f7a6"]
});
console.log(result);
```
{% endtab %}
{% endtabs %}
