---
description: >-
  Example code for the eth_getTransactionByBlockNumberAndIndex JSON-RPC method.
  Complete guide on how to use eth_getTransactionByBlockNumberAndIndex JSON-RPC
  in GetBlock Web3 documentation.
---

# eth\_getTransactionByBlockNumberAndIndex - Mantle

This method returns information about a transaction by block number and transaction index position on the Mantle network.

### Parameters

| Parameter        | Type   | Description                                             |
| ---------------- | ------ | ------------------------------------------------------- |
| blockNumber      | string | Block number in hex, or "latest", "earliest", "pending" |
| transactionIndex | string | Transaction index position (hex)                        |

### Request

{% tabs %}
{% tab title="cURL" %}
{% code title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByBlockNumberAndIndex",
    "params": ["0x3F6777", "0x0"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="JavaScript (axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByBlockNumberAndIndex",
    "params": ["0x3F6777", "0x0"],
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

{% tab title="Request" %}
{% code title="Python (requests)" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByBlockNumberAndIndex",
    "params": ["0x3F6777", "0x0"],
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

{% tab title="Rust" %}
{% code title="Rust (reqwest)" %}
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
            "method": "eth_getTransactionByBlockNumberAndIndex",
            "params": ["0x3F6777", "0x0"],
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
        "value": "0xde0b6b3a7640000"
    }
}
```

### Response Parameters

| Field            | Type   | Description              |
| ---------------- | ------ | ------------------------ |
| hash             | string | Transaction hash         |
| blockHash        | string | Block hash               |
| blockNumber      | string | Block number (hex)       |
| from             | string | Sender address           |
| to               | string | Recipient address        |
| transactionIndex | string | Index in the block       |
| value            | string | Value transferred in wei |

### Use Case

The `eth_getTransactionByBlockNumberAndIndex` method is essential for:

* Sequential block scanning
* Transaction indexing
* Historical data extraction
* Block explorer functionality
* Data synchronization
* Transaction ordering analysis

### Error Handling

| Status Code | Error Message  | Cause                           |
| ----------- | -------------- | ------------------------------- |
| 403         | Forbidden      | Missing or invalid ACCESS-TOKEN |
| -32602      | Invalid params | Invalid block number or index   |

### Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const tx = await provider.send('eth_getTransactionByBlockNumberAndIndex', [
    '0x3F6777',
    '0x0'
]);
console.log('Transaction:', tx);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { mantle } from 'viem/chains';

const client = createPublicClient({
    chain: mantle,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const tx = await client.request({
    method: 'eth_getTransactionByBlockNumberAndIndex',
    params: ['0x3F6777', '0x0']
});
console.log('Transaction:', tx);
```
{% endcode %}
{% endtab %}
{% endtabs %}
