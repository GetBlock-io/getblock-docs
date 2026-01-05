---
description: >-
  Example code for the eth_getTransactionCount JSON-RPC method. Complete guide
  on how to use eth_getTransactionCount JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getTransactionCount - Mantle

This method returns the number of transactions sent from an address (nonce) on the Mantle network.

### Parameters

| Parameter      | Type   | Description                                             |
| -------------- | ------ | ------------------------------------------------------- |
| address        | string | 20-byte address                                         |
| blockParameter | string | Block number in hex, or "latest", "earliest", "pending" |

### Request examples

{% tabs %}
{% tab title="curl" %}
{% code title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getTransactionCount",
    "params": ["0x52988D3DD2D1b36E9c48866670b7683C42197139", "latest"],
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
    "method": "eth_getTransactionCount",
    "params": ["0x52988D3DD2D1b36E9c48866670b7683C42197139", "latest"],
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
    "method": "eth_getTransactionCount",
    "params": ["0x52988D3DD2D1b36E9c48866670b7683C42197139", "latest"],
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
            "method": "eth_getTransactionCount",
            "params": ["0x52988D3DD2D1b36E9c48866670b7683C42197139", "latest"],
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
    "result": "0x2d9a"
}
```
{% endcode %}

### Response Parameters

| Field   | Type   | Description                              |
| ------- | ------ | ---------------------------------------- |
| jsonrpc | string | JSON-RPC protocol version ("2.0")        |
| id      | string | Request identifier matching the request  |
| result  | string | Transaction count (nonce) in hexadecimal |

### Use Case

The `eth_getTransactionCount` method is essential for:

* Nonce management for transaction signing
* Transaction sequencing
* Pending transaction detection
* Account activity analysis
* Wallet transaction preparation
* Replay attack prevention

### Error Handling

| Status Code | Error Message  | Cause                           |
| ----------- | -------------- | ------------------------------- |
| 403         | Forbidden      | Missing or invalid ACCESS-TOKEN |
| -32602      | Invalid params | Invalid address format          |

### Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const nonce = await provider.getTransactionCount('0x52988D3DD2D1b36E9c48866670b7683C42197139');
console.log('Transaction count (nonce):', nonce);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem.js" overflow="wrap" %}
```javascript
import { createPublicClient, http } from 'viem';
import { mantle } from 'viem/chains';

const client = createPublicClient({
    chain: mantle,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const nonce = await client.getTransactionCount({
    address: '0x52988D3DD2D1b36E9c48866670b7683C42197139'
});
console.log('Transaction count (nonce):', nonce);
```
{% endcode %}
{% endtab %}
{% endtabs %}
