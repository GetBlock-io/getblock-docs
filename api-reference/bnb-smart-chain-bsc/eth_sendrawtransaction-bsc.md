---
description: >-
  Example code for the eth_sendRawTransaction JSON RPC method. Сomplete guide on
  how to use eth_sendRawTransaction JSON RPC in GetBlock Web3 documentation.
---

# eth\_sendRawTransaction - BSC

This method submits a pre-signed transaction to the BNB Smart Chain network for execution. This is the primary method for broadcasting transactions and is essential for any application that needs to send BNB or interact with smart contracts.

## Parameters

| Parameter    | Type   | Required | Description                       |
| ------------ | ------ | -------- | --------------------------------- |
| signedTxData | string | Yes      | The signed transaction data (hex) |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_sendRawTransaction",
    "params": ["0xf86c808504a817c80082520894..."],
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
    method: 'eth_sendRawTransaction',
    params: ['0xf86c808504a817c80082520894...'], // Signed tx data
    id: 'getblock.io'
};

axios.post(url, payload, {
    headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log('Tx Hash:', response.data.result))
.catch(error => console.error(error));
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "eth_sendRawTransaction",
    "params": ["0xf86c808504a817c80082520894..."],
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
        "method": "eth_sendRawTransaction",
        "params": ["0xf86c808504a817c80082520894..."],
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
    "result": "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"
}
```

## Response Parameters

| Parameter | Type   | Description                   |
| --------- | ------ | ----------------------------- |
| result    | string | Transaction hash for tracking |

## Use Cases

* Send BNB transfers
* Execute BEP-20 token transfers
* Interact with DeFi protocols
* Deploy smart contracts
* Execute PancakeSwap trades

## Error Handling

| Error Code | Description                            |
| ---------- | -------------------------------------- |
| -32602     | Invalid params - malformed transaction |
| -32000     | Insufficient funds for gas             |
| -32000     | Nonce too low                          |
| -32000     | Replacement transaction underpriced    |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');
const wallet = new ethers.Wallet(privateKey, provider);

const tx = await wallet.sendTransaction({
    to: '0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45',
    value: ethers.parseEther('0.1')
});
console.log('Tx Hash:', tx.hash);
await tx.wait();
console.log('Confirmed!');
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createWalletClient, http, parseEther } from 'viem';
import { bsc } from 'viem/chains';
import { privateKeyToAccount } from 'viem/accounts';

const account = privateKeyToAccount('0x...');

const client = createWalletClient({
    account,
    chain: bsc,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const hash = await client.sendTransaction({
    to: '0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45',
    value: parseEther('0.1')
});
console.log('Tx Hash:', hash);
```
{% endtab %}
{% endtabs %}
