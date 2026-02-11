---
description: >-
  Example code for the eth_getCode JSON RPC method. Сomplete guide on how to use
  eth_getCode JSON RPC in GetBlock Web3 documentation.
---

# eth\_getCode - BSC

This method returns the bytecode stored at a given address on the BNB Smart Chain. This is useful for verifying contract deployments, distinguishing between EOAs and contracts, and analyzing smart contract code.

## Parameters

| Parameter   | Type   | Required | Description                                             |
| ----------- | ------ | -------- | ------------------------------------------------------- |
| address     | string | Yes      | Contract address to query                               |
| blockNumber | string | Yes      | Block number in hex, or "latest", "earliest", "pending" |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getCode",
    "params": ["0x55d398326f99059fF775485246999027B3197955", "latest"],
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

// USDT contract on BSC
const payload = {
    jsonrpc: '2.0',
    method: 'eth_getCode',
    params: ['0x55d398326f99059fF775485246999027B3197955', 'latest'],
    id: 'getblock.io'
};

axios.post(url, payload, {
    headers: { 'Content-Type': 'application/json' }
})
.then(response => {
    const code = response.data.result;
    console.log('Is Contract:', code !== '0x');
    console.log('Code Length:', (code.length - 2) / 2, 'bytes');
})
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
    "method": "eth_getCode",
    "params": ["0x55d398326f99059fF775485246999027B3197955", "latest"],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
code = response.json()["result"]
print(f"Is Contract: {code != '0x'}")
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
        "method": "eth_getCode",
        "params": ["0x55d398326f99059fF775485246999027B3197955", "latest"],
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

{% code title="response.json" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x608060405234801561001057600080fd5b50600436106100..."
}
```
{% endcode %}

## Response Parameters

| Parameter | Type   | Description                                       |
| --------- | ------ | ------------------------------------------------- |
| result    | string | Bytecode (0x if EOA, actual bytecode if contract) |

## Use Cases

* Verify contract deployment on BSC
* Distinguish contracts from EOAs
* Analyze BEP-20 token contracts
* Validate proxy implementations
* Security auditing

## Error Handling

| Error Code | Description                        |
| ---------- | ---------------------------------- |
| -32602     | Invalid params - malformed address |
| -32603     | Internal error                     |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const code = await provider.getCode('0x55d398326f99059fF775485246999027B3197955');
console.log('Is Contract:', code !== '0x');
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { bsc } from 'viem/chains';

const client = createPublicClient({
    chain: bsc,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const code = await client.getCode({
    address: '0x55d398326f99059fF775485246999027B3197955'
});
console.log('Is Contract:', code !== undefined && code !== '0x');
```
{% endcode %}
{% endtab %}
{% endtabs %}
