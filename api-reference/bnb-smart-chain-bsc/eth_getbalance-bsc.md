---
description: >-
  Example code for the eth_getBalance JSON RPC method. Сomplete guide on how to
  use eth_getBalance JSON RPC in GetBlock Web3 documentation.
---

# eth\_getBalance - BSC

This method returns the balance of the account at a given address on the BNB Smart Chain. The balance is returned in wei (the smallest unit of BNB, where 1 BNB = 10^18 wei). This method is essential for wallet applications, balance verification, and transaction preparation.

{% hint style="info" %}
Balances are returned in wei (1 BNB = 10^18 wei). Convert from hex wei to decimal and then to BNB by dividing by 1e18.
{% endhint %}

## Parameters

| Parameter | Type   | Required | Description                                             |
| --------- | ------ | -------- | ------------------------------------------------------- |
| address   | string | Yes      | The address to check balance (20 bytes, 0x-prefixed)    |
| block     | string | Yes      | Block number in hex, or "latest", "earliest", "pending" |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBalance",
    "params": ["0x8D97689C9818892B700e27F316cc3E41e17fBeb9", "latest"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Javascript (Axios)" %}
{% code title="JavaScript (Axios)" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'eth_getBalance',
    params: ['0x8D97689C9818892B700e27F316cc3E41e17fBeb9', 'latest'],
    id: 'getblock.io'
};

axios.post(url, payload, {
    headers: { 'Content-Type': 'application/json' }
})
.then(response => {
    const balanceWei = parseInt(response.data.result, 16);
    const balanceBnb = balanceWei / 1e18;
    console.log('Balance:', balanceBnb, 'BNB');
})
.catch(error => console.error(error));
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="Python" overflow="wrap" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "eth_getBalance",
    "params": ["0x8D97689C9818892B700e27F316cc3E41e17fBeb9", "latest"],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
balance_wei = int(response.json()["result"], 16)
balance_bnb = balance_wei / 1e18
print(f"Balance: {balance_bnb} BNB")
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="Rust" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let payload = json!({
        "jsonrpc": "2.0",
        "method": "eth_getBalance",
        "params": ["0x8D97689C9818892B700e27F316cc3E41e17fBeb9", "latest"],
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

{% code title="Response (JSON)" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x8ac7230489e80000"
}
```
{% endcode %}

## Response Parameters

| Parameter | Type   | Description                                  |
| --------- | ------ | -------------------------------------------- |
| jsonrpc   | string | JSON-RPC version (2.0)                       |
| id        | string | Request identifier                           |
| result    | string | Balance in wei (0x8ac7230489e80000 = 10 BNB) |

## Use Cases

* Display wallet balance in dApps
* Verify sufficient BNB for gas fees
* Check account holdings before transactions
* Monitor wallet addresses
* Calculate portfolio values

## Error Handling

| Error Code | Description                                           |
| ---------- | ----------------------------------------------------- |
| -32602     | Invalid params - malformed address or block parameter |
| -32603     | Internal error - node processing issues               |
| -32000     | Server error - RPC endpoint unavailable               |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="Ethers.js" overflow="wrap" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const balance = await provider.getBalance('0x8D97689C9818892B700e27F316cc3E41e17fBeb9');
console.log('Balance:', ethers.formatEther(balance), 'BNB');
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="Viem" %}
```javascript
import { createPublicClient, http, formatEther } from 'viem';
import { bsc } from 'viem/chains';

const client = createPublicClient({
    chain: bsc,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const balance = await client.getBalance({
    address: '0x8D97689C9818892B700e27F316cc3E41e17fBeb9'
});
console.log('Balance:', formatEther(balance), 'BNB');
```
{% endcode %}
{% endtab %}
{% endtabs %}
