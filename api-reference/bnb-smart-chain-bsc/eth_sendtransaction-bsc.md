---
description: >-
  Example code for the eth_sendTransaction JSON RPC method. Сomplete guide on
  how to use eth_sendTransaction JSON RPC in GetBlock Web3 documentation.
---

# eth\_sendTransaction - BSC

This method requires the sender account to be unlocked on the node. This method is not available on public RPC endpoints (for example GetBlock). For public RPCs, use eth\_sendRawTransaction with a locally signed transaction.

## Parameters

| Parameter   | Type   | Required | Description        |
| ----------- | ------ | -------- | ------------------ |
| transaction | object | Yes      | Transaction object |

### Transaction Object

| Field    | Type   | Required | Description       |
| -------- | ------ | -------- | ----------------- |
| from     | string | Yes      | Sender address    |
| to       | string | No       | Recipient address |
| gas      | string | No       | Gas limit         |
| gasPrice | string | No       | Gas price         |
| value    | string | No       | Value in wei      |
| data     | string | No       | Contract data     |
| nonce    | string | No       | Transaction nonce |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_sendTransaction",
    "params": [{
        "from": "0x8D97689C9818892B700e27F316cc3E41e17fBeb9",
        "to": "0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45",
        "value": "0xde0b6b3a7640000"
    }],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="request.js" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'eth_sendTransaction',
    params: [{
        from: '0x8D97689C9818892B700e27F316cc3E41e17fBeb9',
        to: '0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45',
        value: '0xde0b6b3a7640000'
    }],
    id: 'getblock.io'
};

axios.post(url, payload, {
    headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log(response.data))
.catch(error => console.error(error));
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="request.py" %}
```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "eth_sendTransaction",
    "params": [{
        "from": "0x8D97689C9818892B700e27F316cc3E41e17fBeb9",
        "to": "0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45",
        "value": "0xde0b6b3a7640000"
    }],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="main.rs" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let payload = json!({
        "jsonrpc": "2.0",
        "method": "eth_sendTransaction",
        "params": [{
            "from": "0x8D97689C9818892B700e27F316cc3E41e17fBeb9",
            "to": "0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45",
            "value": "0xde0b6b3a7640000"
        }],
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
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

```json
{
 "jsonrpc": "2.0",
 "id": "getblock.io",
 "result": "0x4f7d1c3f6def3e6b1b98c3044818f60e5fa588f1d6bb64df776c6fcf51244b91"
}
```

## Response Parameters

| Parameter | Type   | Description                     |
| --------- | ------ | ------------------------------- |
| result    | string | Transaction hash (if supported) |

## Use Cases

* Local node development
* Private node operations
* Testing environments

## Error Handling

| Error Code | Description                        |
| ---------- | ---------------------------------- |
| -32601     | Method not supported (public RPCs) |
| -32000     | Account not unlocked               |
| -32602     | Invalid params                     |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" %}
```javascript
const { ethers } = require('ethers');

// Use eth_sendRawTransaction instead for public RPCs
const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');
const wallet = new ethers.Wallet(privateKey, provider);

const tx = await wallet.sendTransaction({
    to: '0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45',
    value: ethers.parseEther('1.0')
});
console.log('Tx Hash:', tx.hash);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem.js" %}
```javascript
import { createWalletClient, http, parseEther } from 'viem';
import { bsc } from 'viem/chains';
import { privateKeyToAccount } from 'viem/accounts';

// Use wallet client with private key for public RPCs
const account = privateKeyToAccount('0x...');

const client = createWalletClient({
    account,
    chain: bsc,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const hash = await client.sendTransaction({
    to: '0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45',
    value: parseEther('1.0')
});
```
{% endcode %}
{% endtab %}
{% endtabs %}
