---
description: >-
  Example code for the eth_getTransactionByHash JSON-RPC method. Complete guide
  on how to use eth_getTransactionByHash JSON-RPC in GetBlock Web3
  documentation.
---

# eth\_getTransactionByHash - Mantle

This method returns information about a transaction on Mantle by transaction hash.

### Parameters

| Parameter       | Type   | Description              |
| --------------- | ------ | ------------------------ |
| transactionHash | string | 32-byte transaction hash |

### Request

{% tabs %}
{% tab title="curl" %}
{% code title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByHash",
    "params": ["0x6dfef681e0a83a40b05d40877e53a88459e8829240a6d6d5ec6fe816e435f8d8"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (axios)" %}
{% code title="example.js" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByHash",
    "params": ["0x6dfef681e0a83a40b05d40877e53a88459e8829240a6d6d5ec6fe816e435f8d8"],
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
{% code title="example.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByHash",
    "params": ["0x6dfef681e0a83a40b05d40877e53a88459e8829240a6d6d5ec6fe816e435f8d8"],
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
{% code title="main.rs" %}
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
            "method": "eth_getTransactionByHash",
            "params": ["0x6dfef681e0a83a40b05d40877e53a88459e8829240a6d6d5ec6fe816e435f8d8"],
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

{% code title="response.json" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "blockHash": "0x9b14d73f45c836bfb0e1f59453c39fe8cddab45a0f78670dc6190e5c85b65a8e",
        "blockNumber": "0x3f6777",
        "from": "0x52988D3DD2D1b36E9c48866670b7683C42197139",
        "gas": "0x5208",
        "gasPrice": "0x1",
        "hash": "0x6dfef681e0a83a40b05d40877e53a88459e8829240a6d6d5ec6fe816e435f8d8",
        "input": "0x",
        "nonce": "0x2d99",
        "to": "0xD85498dbEaEB1Df24BE52eED4F52eAc2Fbd56245",
        "transactionIndex": "0x0",
        "value": "0xde0b6b3a7640000",
        "v": "0x1",
        "r": "0x...",
        "s": "0x..."
    }
}
```
{% endcode %}

### Response Parameters

| Field       | Type   | Description                            |
| ----------- | ------ | -------------------------------------- |
| hash        | string | Transaction hash                       |
| blockHash   | string | Block hash containing this transaction |
| blockNumber | string | Block number (hex)                     |
| from        | string | Sender address                         |
| to          | string | Recipient address                      |
| value       | string | Value transferred in wei               |
| gas         | string | Gas provided                           |
| gasPrice    | string | Gas price in wei                       |
| input       | string | Transaction input data                 |
| nonce       | string | Sender's nonce                         |

### Use Case

The `eth_getTransactionByHash` method is essential for:

* Transaction status verification
* Block explorer functionality
* Payment confirmation
* Transaction debugging
* Wallet transaction history
* DeFi transaction tracking

### Error Handling

| Status Code | Error Message  | Cause                           |
| ----------- | -------------- | ------------------------------- |
| 403         | Forbidden      | Missing or invalid ACCESS-TOKEN |
| -32602      | Invalid params | Invalid transaction hash format |

### Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const tx = await provider.getTransaction('0x6dfef681e0a83a40b05d40877e53a88459e8829240a6d6d5ec6fe816e435f8d8');
console.log('Transaction:', tx);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { mantle } from 'viem/chains';

const client = createPublicClient({
    chain: mantle,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const tx = await client.getTransaction({
    hash: '0x6dfef681e0a83a40b05d40877e53a88459e8829240a6d6d5ec6fe816e435f8d8'
});
console.log('Transaction:', tx);
```
{% endcode %}
{% endtab %}
{% endtabs %}
