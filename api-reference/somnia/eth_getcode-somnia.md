---
description: >-
  Example code for the eth_getCode JSON-RPC method. Complete guide on how to use
  eth_getCode JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getCode - Somnia

This method returns the bytecode at a given address on the Somnia network. This is useful for verifying contract deployment, checking if an address is a contract, and analyzing contract bytecode. Somnia's compiled EVM executes this bytecode at near-native speeds.

## Parameters

| Parameter   | Type   | Required | Description                                             |
| ----------- | ------ | -------- | ------------------------------------------------------- |
| address     | string | Yes      | The address to get code from                            |
| blockNumber | string | Yes      | Block number in hex, or "latest", "earliest", "pending" |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "eth_getCode",
  "params": [
    "0xContractAddress",
    "latest"
  ]
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
  method: 'eth_getCode',
  params: ['0xContractAddress', 'latest']
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
    "method": "eth_getCode",
    "params": ["0xContractAddress", "latest"]
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
        "method": "eth_getCode",
        "params": ["0xContractAddress", "latest"]
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

{% code overflow="wrap" %}
```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": "0x608060405234801561001057600080fd5b50600436106100365760003560e01c806370a082311461003b578063a9059cbb14610069575b600080fd5b..."
}
```
{% endcode %}

## Response Parameters

| Parameter | Type   | Description                              |
| --------- | ------ | ---------------------------------------- |
| result    | string | Contract bytecode (0x if not a contract) |

## Use Cases

* Verify contract deployment
* Check if address is a contract vs EOA
* Analyze contract bytecode
* Detect proxy contracts
* Security auditing

## Error Handling

| Error Code | Description                             |
| ---------- | --------------------------------------- |
| -32602     | Invalid params - malformed address      |
| -32603     | Internal error - node processing issues |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const code = await provider.getCode('0xContractAddress');
console.log(code);
// "0x" means it's an EOA, otherwise it's a contract
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { somnia } from 'viem/chains';

const client = createPublicClient({
  chain: somnia,
  transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const code = await client.getCode({
  address: '0xContractAddress'
});
console.log(code);
```
{% endtab %}
{% endtabs %}
