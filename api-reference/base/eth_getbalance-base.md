---
description: >-
  Example code for the eth_getBalance JSON-RPC method. Complete guide on how to
  use eth_getBalance JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getBalance - Base

This method returns the ETH balance of a specified address on the Base network. Since Base uses ETH as its native gas token, this method returns the native token balance in wei.

## Parameters

| Parameter      | Type   | Required | Description                                             |
| -------------- | ------ | -------- | ------------------------------------------------------- |
| address        | string | Yes      | The 20-byte address to check the balance of             |
| blockParameter | string | Yes      | Block number in hex, or "latest", "earliest", "pending" |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBalance",
    "params": ["0x4200000000000000000000000000000000000006", "latest"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code overflow="wrap" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({{
    "jsonrpc": "2.0",
    "method": "eth_getBalance",
    "params": ["0x4200000000000000000000000000000000000006", "latest"],
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
{% code overflow="wrap" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_getBalance",
    "params": ["0x4200000000000000000000000000000000000006", "latest"],
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
{% code overflow="wrap" %}
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
    "method": "eth_getBalance",
    "params": ["0x4200000000000000000000000000000000000006", "latest"],
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

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x56bc75e2d63100000"
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| id        | string | Request identifier matching the request |
| result    | string | Hexadecimal balance in wei              |

## Use Cases

1. **Wallet Balance Display:** Show user's ETH balance in wallet interfaces.
2. **Transaction Validation:** Verify sufficient balance before sending transactions.
3. **Portfolio Tracking:** Aggregate balances across multiple addresses.
4. **Smart Contract Verification:** Check contract balances for DeFi applications.
5. **Historical Analysis:** Query balances at specific block heights.

## Error Handling

| Error Code | Message         | Description                               |
| ---------- | --------------- | ----------------------------------------- |
| -32602     | Invalid params  | Invalid address format or block parameter |
| -32603     | Internal error  | Node internal failure                     |
| -32600     | Invalid request | Malformed JSON-RPC request                |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

async function getBalance(address) {
    const balance = await provider.getBalance(address);
    
    console.log('Balance (wei):', balance.toString());
    console.log('Balance (ETH):', ethers.formatEther(balance));
    
    return balance;
}

// Check WETH contract balance on Base
getBalance('0x4200000000000000000000000000000000000006');
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http, formatEther } from 'viem';
import { base } from 'viem/chains';

const client = createPublicClient({
    chain: base,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

async function getBalance(address) {
    const balance = await client.getBalance({
        address: address
    });
    
    console.log('Balance (wei):', balance.toString());
    console.log('Balance (ETH):', formatEther(balance));
    
    return balance;
}

// Check WETH contract balance on Base
getBalance('0x4200000000000000000000000000000000000006');
```
{% endtab %}
{% endtabs %}
