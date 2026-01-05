---
description: >-
  Example code for the eth_call JSON-RPC method. Complete guide on how to use
  eth_call JSON-RPC in GetBlock Web3 documentation.
---

# eth\_call - Mantle

This method executes a new message call immediately without creating a transaction on the Mantle blockchain. Useful for reading smart contract data.

### Parameters

| Parameter      | Type   | Description                                             |
| -------------- | ------ | ------------------------------------------------------- |
| transaction    | object | Transaction call object                                 |
| blockParameter | string | Block number in hex, or "latest", "earliest", "pending" |

**Transaction Object:**

| Field    | Type   | Description                           |
| -------- | ------ | ------------------------------------- |
| from     | string | (optional) Sender address             |
| to       | string | Contract address to call              |
| gas      | string | (optional) Gas provided for execution |
| gasPrice | string | (optional) Gas price in wei           |
| value    | string | (optional) Value sent with call       |
| data     | string | (optional) Encoded function call data |

### Request

{% tabs %}
{% tab title="curl" %}
{% code title="curl" overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_call",
    "params": [{
        "to": "0x6b175474e89094c44da98b954eedeac495271d0f",
        "data": "0x70a082310000000000000000000000006E0d01A76C3Cf4288372a29124A26D4353EE51BE"
    }, "latest"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (axios)" %}
{% code title="request.js" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_call",
    "params": [{
        "to": "0x6b175474e89094c44da98b954eedeac495271d0f",
        "data": "0x70a082310000000000000000000000006E0d01A76C3Cf4288372a29124A26D4353EE51BE"
    }, "latest"],
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
{% code title="request.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_call",
    "params": [{
        "to": "0x6b175474e89094c44da98b954eedeac495271d0f",
        "data": "0x70a082310000000000000000000000006E0d01A76C3Cf4288372a29124A26D4353EE51BE"
    }, "latest"],
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
            "method": "eth_call",
            "params": [{
                "to": "0x6b175474e89094c44da98b954eedeac495271d0f",
                "data": "0x70a082310000000000000000000000006E0d01A76C3Cf4288372a29124A26D4353EE51BE"
            }, "latest"],
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
    "result": "0x0000000000000000000000000000000000000000000000000858898f93629000"
}
```

### Response Parameters

| Field   | Type   | Description                                          |
| ------- | ------ | ---------------------------------------------------- |
| jsonrpc | string | JSON-RPC protocol version ("2.0")                    |
| id      | string | Request identifier matching the request              |
| result  | string | Return value of executed contract call (hex encoded) |

### Use Case

The `eth_call` method is essential for:

* Reading ERC-20 token balances
* Querying smart contract state
* Simulating transactions before execution
* DeFi protocol data retrieval
* NFT metadata queries
* Price oracle data fetching

### Error Handling

| Status Code | Error Message      | Cause                                         |
| ----------- | ------------------ | --------------------------------------------- |
| 403         | Forbidden          | Missing or invalid ACCESS-TOKEN               |
| -32602      | Invalid params     | Invalid transaction object or block parameter |
| -32000      | Execution reverted | Contract execution failed                     |

### Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const result = await provider.call({
    to: '0x6b175474e89094c44da98b954eedeac495271d0f',
    data: '0x70a082310000000000000000000000006E0d01A76C3Cf4288372a29124A26D4353EE51BE'
});
console.log('Result:', result);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { mantle } from 'viem/chains';

const client = createPublicClient({
    chain: mantle,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const result = await client.call({
    to: '0x6b175474e89094c44da98b954eedeac495271d0f',
    data: '0x70a082310000000000000000000000006E0d01A76C3Cf4288372a29124A26D4353EE51BE'
});
console.log('Result:', result);
```
{% endcode %}
{% endtab %}
{% endtabs %}
