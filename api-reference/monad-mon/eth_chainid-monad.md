---
description: >-
  Example code for the eth_chainId JSON-RPC method. Complete guide on how to use
  eth_chainId JSON-RPC in GetBlock Web3 documentation.
---

# eth\_chainId - Monad

This method returns the chain ID used for signing replay-protected transactions.

## Parameters

* None

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_chainId",
    "params": [],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_chainId",
    "params": [],
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
    "method": "eth_chainId",
    "params": [],
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
            "method": "eth_chainId",
            "params": [],
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

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x8f"
}
```

## Response Parameters

| Field  | Type   | Description                                                |
| ------ | ------ | ---------------------------------------------------------- |
| result | string | The chain ID in hexadecimal. 0x8f = 143 for Monad Mainnet. |

## Monad Network Chain IDs

| Network       | Chain ID (Decimal) | Chain ID (Hex) |
| ------------- | -----------------: | -------------- |
| Monad Mainnet |                143 | 0x8f           |
| Monad Testnet |              10143 | 0x279f         |

## Use Case

The `eth_chainId` method is essential for:

* Transaction signing with replay protection
* Network verification before transactions
* Multi-chain wallet integration
* dApp network detection
* Preventing cross-chain replay attacks
* MetaMask and wallet configuration

## Error Handling

| Status Code | Error Message | Cause                            |
| ----------- | ------------- | -------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS-TOKEN. |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const network = await provider.getNetwork();
console.log('Chain ID:', network.chainId.toString()); // 143

// Verify connected to Monad Mainnet
if (network.chainId === 143n) {
    console.log('Connected to Monad Mainnet');
}
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { monad } from 'viem/chains';

// Create Viem client with GetBlock
const client = createPublicClient({
  chain: monad,
  transport: http("https://go.getblock.us/<ACCESS_TOKEN>"),
});

// Using the method through Viem
async function fetchAccountViem() {
    try {
        // Method-specific Viem implementation
        const result = await client.request({
            method: 'eth_chainId',
            param: [],
        });
        console.log('Result:', result);
        return result;
    } catch (error) {
        console.error('Viem Error:', error);
        throw error;
    }
}
```
{% endtab %}
{% endtabs %}
