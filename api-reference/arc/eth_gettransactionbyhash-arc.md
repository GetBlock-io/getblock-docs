---
description: >-
  Example code for the eth_getTransactionByHash JSON-RPC method. Complete guide
  on how to use eth_getTransactionByHash JSON-RPC in GetBlock Web3
  documentation.
---

# eth\_getTransactionByHash - ARC

This method returns transaction details by transaction hash. Useful for inspecting any transaction once you have its hash from a broadcast or a block listing. Note that all USDC amounts in the response (`value`, `gasPrice`, `maxFeePerGas`) are in USDC base units, not 18-decimal wei.

## Parameters

| Parameter       | Type   | Required | Description                     |
| --------------- | ------ | -------- | ------------------------------- |
| transactionHash | string | Yes      | 32-byte hash of the transaction |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByHash",
    "params": [
        "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"
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
    "method": "eth_getTransactionByHash",
    "params": [
        "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"
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
    "method": "eth_getTransactionByHash",
    "params": [
        "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"
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
        "method": "eth_getTransactionByHash",
        "params": [
                "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"
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
        "blockHash": "0x4f3a1d6e8c2b9a7e5d3f1c8a4b6e9d2f5a8c3e7b1d4f9a6c2e5b8d3f7a1c4e9b",
        "blockNumber": "0xa7d8c",
        "from": "0x4cef52a8f9d4b0c8b8d5f1a2b3c4d5e6f7a8b9c0",
        "gas": "0x5208",
        "gasPrice": "0x3b9aca00",
        "maxFeePerGas": "0x77359400",
        "maxPriorityFeePerGas": "0x3b9aca00",
        "hash": "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b",
        "input": "0x",
        "nonce": "0x42",
        "to": "0xd85498dbeaeb1df24be52eed4f52eac2fbd56245",
        "transactionIndex": "0x0",
        "value": "0xf4240",
        "type": "0x2",
        "chainId": "0x4cef52",
        "v": "0x0",
        "r": "0x4f3a1c\u2026",
        "s": "0x6c8d2b\u2026"
    }
}
```
{% endcode %}

## Response Parameters

| Field                       | Type           | Description                                                                    |
| --------------------------- | -------------- | ------------------------------------------------------------------------------ |
| result.blockHash            | string \| null | Hash of the block containing this transaction, or `null` if pending            |
| result.blockNumber          | string \| null | Block number, or `null` if pending                                             |
| result.from                 | string         | Sender address                                                                 |
| result.to                   | string \| null | Recipient address; `null` for contract creations                               |
| result.value                | string         | USDC transferred in base units (hex). `0xf4240` = 1,000,000 = 1 USDC           |
| result.gas                  | string         | Gas limit (hex)                                                                |
| result.gasPrice             | string         | Legacy gas price in USDC base units (hex)                                      |
| result.maxFeePerGas         | string         | Max total fee per gas (EIP-1559, USDC base units)                              |
| result.maxPriorityFeePerGas | string         | Max priority fee per gas (EIP-1559, USDC base units)                           |
| result.input                | string         | Calldata (hex)                                                                 |
| result.nonce                | string         | Sender nonce (hex)                                                             |
| result.chainId              | string         | Chain ID (`0x4cef52` for Arc testnet)                                          |
| result.type                 | string         | Transaction type: `0x0` legacy, `0x1` EIP-2930, `0x2` EIP-1559, `0x4` EIP-7702 |

## Use Cases

* Inspecting a USDC payment after broadcast
* Building transaction detail views in wallets and explorers
* Decoding calldata for known contract interfaces (USDC, FX engine, etc.)
* Confirming transaction inclusion vs pending status

## Error Handling

| Status Code | Error Message     | Cause                                       |
| ----------- | ----------------- | ------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`         |
| -32602      | Invalid params    | Request parameters are missing or malformed |
| -32601      | Method not found  | The method is not supported by this node    |
| 429         | Too Many Requests | Rate limit exceeded for your plan           |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// Most methods are exposed directly on the provider.
// For raw access, use provider.send(method, params).
const result = await provider.send('eth_getTransactionByHash', ["0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"]);
console.log(result);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code overflow="wrap" %}
```javascript
import { createPublicClient, http, defineChain } from 'viem';

const arcTestnet = defineChain({
    id: 5042002,
    name: 'Arc Testnet',
    network: 'arc-testnet',
    nativeCurrency: { name: 'USD Coin', symbol: 'USDC', decimals: 6 },
    rpcUrls: { default: { http: ['https://go.getblock.io/<ACCESS-TOKEN>/'] } },
    blockExplorers: { default: { name: 'arcscan', url: 'https://testnet.arcscan.app' } }
});

const client = createPublicClient({
    chain: arcTestnet,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const result = await client.request({ method: 'eth_getTransactionByHash', params: ["0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"] });
console.log(result);
```
{% endcode %}
{% endtab %}
{% endtabs %}
