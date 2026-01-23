---
description: >-
  Example code for the eth_chainId JSON-RPC method. Complete guide on how to use
  eth_chainId JSON-RPC in GetBlock Web3 documentation.
---

# eth\_chainId - Celo

This method returns the chain ID of the Celo network. The chain ID is used to prevent replay attacks across different EVM networks. Celo Mainnet uses chain ID 42220, while the Alfajores Testnet uses 44787. This value is essential for transaction signing and network identification.

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "eth_chainId",
  "params": []
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'eth_chainId',
  params: []
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
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "eth_chainId",
    "params": []
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
        "id": "getblock.io",
        "method": "eth_chainId",
        "params": []
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
  "result": "0xa4ec"
}
```

## Response Definition

| Parameter | Type   | Description                                  |
| --------- | ------ | -------------------------------------------- |
| result    | string | Chain ID in hex (0xa4ec = 42220 for Mainnet) |

## Use Cases

* Verify connection to correct network
* Include in transaction signing for replay protection
* Validate RPC endpoint configuration
* Network detection in multi-chain applications
* Wallet network switching

## Error Handling

| Error Code | Description                             |
| ---------- | --------------------------------------- |
| -32603     | Internal error - node processing issues |
| -32000     | Server error - RPC endpoint unavailable |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const network = await provider.getNetwork();
console.log(network.chainId); // 42220n for Mainnet
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { celo } from 'viem/chains';

const client = createPublicClient({
  chain: celo,
  transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const chainId = await client.getChainId();
console.log(chainId); // 42220 for Mainnet
```
{% endtab %}

{% tab title="ContractKit" %}
```javascript
const { newKit } = require('@celo/contractkit');

const kit = newKit('https://go.getblock.io/<ACCESS-TOKEN>/');

const chainId = await kit.web3.eth.getChainId();
console.log(chainId); // 42220 for Mainnet
```
{% endtab %}
{% endtabs %}
