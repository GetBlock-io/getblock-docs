---
description: >-
  Example code for the eth_getTransactionByHash JSON RPC method. Сomplete guide
  on how to use eth_getTransactionByHash JSON RPC in GetBlock Web3
  documentation.
---

# eth\_getTransactionByHash - BSC

This method returns information about a transaction by its hash on the BNB Smart Chain. This is essential for tracking transaction status and retrieving transaction details.

## Parameters

| Parameter       | Type   | Required | Description              |
| --------------- | ------ | -------- | ------------------------ |
| transactionHash | string | Yes      | 32-byte transaction hash |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByHash",
    "params": ["0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'eth_getTransactionByHash',
    params: ['0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b'],
    id: 'getblock.io'
};

axios.post(url, payload, {
    headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log(response.data))
.catch(error => console.error(error));
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByHash",
    "params": ["0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endtab %}

{% tab title="Rust" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let payload = json!({
        "jsonrpc": "2.0",
        "method": "eth_getTransactionByHash",
        "params": ["0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"],
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

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "hash": "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b",
        "blockHash": "0x4e3a3754410177e6937ef1f84bba68ea139e8d1a2258c5f85db9f1cd715a1bdd",
        "blockNumber": "0x2625a00",
        "from": "0x742d35cc6634c0532925a3b844bc9e7595f8bb45",
        "to": "0x55d398326f99059ff775485246999027b3197955",
        "value": "0x0",
        "gas": "0x5208",
        "gasPrice": "0x12a05f200",
        "input": "0xa9059cbb...",
        "nonce": "0x15"
    }
}
```

## Response Parameters

| Field       | Type   | Description       |
| ----------- | ------ | ----------------- |
| hash        | string | Transaction hash  |
| blockHash   | string | Block hash        |
| blockNumber | string | Block number      |
| from        | string | Sender address    |
| to          | string | Recipient address |
| value       | string | Value in wei      |
| gas         | string | Gas limit         |
| gasPrice    | string | Gas price         |
| input       | string | Input data        |
| nonce       | string | Sender nonce      |

## Use Cases

* Track transaction status
* Display transaction details
* Verify transaction inclusion
* Build wallet transaction history

## Error Handling

| Error Code  | Description           |
| ----------- | --------------------- |
| -32602      | Invalid params        |
| null result | Transaction not found |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code overflow="wrap" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const tx = await provider.getTransaction('0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b');
console.log(tx);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { bsc } from 'viem/chains';

const client = createPublicClient({
    chain: bsc,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const tx = await client.getTransaction({
    hash: '0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b'
});
console.log(tx);
```
{% endtab %}
{% endtabs %}
