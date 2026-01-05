---
description: >-
  Example code for the eth_getCode JSON-RPC method. Complete guide on how to use
  eth_getCode JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getCode - Mantle

This method returns code at a given address on the Mantle network.

### Parameters

| Parameter      | Type   | Description                                             |
| -------------- | ------ | ------------------------------------------------------- |
| address        | string | 20-byte contract address                                |
| blockParameter | string | Block number in hex, or "latest", "earliest", "pending" |

### Request

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getCode",
    "params": ["0x201EBa5CC46D216Ce6DC03F6a759e8E766e956aE", "latest"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_getCode",
    "params": ["0x201EBa5CC46D216Ce6DC03F6a759e8E766e956aE", "latest"],
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
{% endtab %}

{% tab title="Python" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_getCode",
    "params": ["0x201EBa5CC46D216Ce6DC03F6a759e8E766e956aE", "latest"],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endtab %}

{% tab title="Rust" %}
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
            "method": "eth_getCode",
            "params": ["0x201EBa5CC46D216Ce6DC03F6a759e8E766e956aE", "latest"],
            "id": "getblock.io"
        }"#)
        .send()
        .await?;
    
    println!("{}", response.text().await?);
    Ok(())
}
```
{% endtab %}
{% endtabs %}

### Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x608060405234801561001057600080fd5b50..."
}
```

### Response Parameters

| Field   | Type   | Description                                 |
| ------- | ------ | ------------------------------------------- |
| jsonrpc | string | JSON-RPC protocol version ("2.0")           |
| id      | string | Request identifier matching the request     |
| result  | string | Contract bytecode (hex), or "0x" if no code |

### Use Case

The `eth_getCode` method is essential for:

* Contract verification
* Checking if address is a contract
* Smart contract analysis
* Security auditing
* Contract deployment verification
* Distinguishing EOA from contracts

### Error Handling

| Status Code | Error Message  | Cause                           |
| ----------- | -------------- | ------------------------------- |
| 403         | Forbidden      | Missing or invalid ACCESS-TOKEN |
| -32602      | Invalid params | Invalid address format          |

### Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const code = await provider.getCode('0x201EBa5CC46D216Ce6DC03F6a759e8E766e956aE');
console.log('Contract code:', code);
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { mantle } from 'viem/chains';

const client = createPublicClient({
    chain: mantle,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const code = await client.getCode({
    address: '0x201EBa5CC46D216Ce6DC03F6a759e8E766e956aE'
});
console.log('Contract code:', code);
```
{% endtab %}
{% endtabs %}
