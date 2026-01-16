---
description: >-
  Example code for the eth_call JSON-RPC method. Complete guide on how to use
  eth_call JSON-RPC in GetBlock Web3 documentation.
---

# eth\_call - Somnia

This method executes a new message call immediately without creating a transaction on the blockchain. This is commonly used for reading data from smart contracts, simulating transactions, and querying contract state on the Somnia network.

## Parameters

| Parameter   | Type   | Required | Description                                             |
| ----------- | ------ | -------- | ------------------------------------------------------- |
| transaction | object | Yes      | Transaction call object                                 |
| blockNumber | string | Yes      | Block number in hex, or "latest", "earliest", "pending" |

### Transaction Object

| Field    | Type   | Required | Description           |
| -------- | ------ | -------- | --------------------- |
| from     | string | No       | Sender address        |
| to       | string | Yes      | Contract address      |
| gas      | string | No       | Gas limit             |
| gasPrice | string | No       | Gas price             |
| value    | string | No       | Value to send         |
| data     | string | No       | Encoded function call |

## Returns

| Field  | Type   | Description               |
| ------ | ------ | ------------------------- |
| result | string | Return data from the call |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="curl" overflow="wrap" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "eth_call",
  "params": [
    {
      "to": "0xContractAddress",
      "data": "0x70a08231000000000000000000000000742d35cc6634c0532925a3b844bc9e7595f8bb45"
    },
    "latest"
  ]
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="request.js" overflow="wrap" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'eth_call',
  params: [
    {
      to: '0xContractAddress',
      data: '0x70a08231000000000000000000000000742d35cc6634c0532925a3b844bc9e7595f8bb45'
    },
    'latest'
  ]
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
{% code title="request.py" overflow="wrap" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "eth_call",
    "params": [
        {
            "to": "0xContractAddress",
            "data": "0x70a08231000000000000000000000000742d35cc6634c0532925a3b844bc9e7595f8bb45"
        },
        "latest"
    ]
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="request.rs" overflow="wrap" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let payload = json!({
        "jsonrpc": "2.0",
        "id": "getblock.io",
        "method": "eth_call",
        "params": [
            {
                "to": "0xContractAddress",
                "data": "0x70a08231000000000000000000000000742d35cc6634c0532925a3b844bc9e7595f8bb45"
            },
            "latest"
        ]
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
  "result": "0x0000000000000000000000000000000000000000000000000de0b6b3a7640000"
}
```

## Response Parameters

| Parameter | Type   | Description              |
| --------- | ------ | ------------------------ |
| result    | string | ABI-encoded return value |

## Use Cases

* Read ERC-20 token balances
* Query contract state
* Simulate transactions before sending
* Get NFT metadata
* Check allowances and approvals

## Error Handling

| Error Code | Description                            |
| ---------- | -------------------------------------- |
| -32602     | Invalid params - malformed call object |
| -32603     | Internal error - execution reverted    |
| -32000     | Execution error - contract revert      |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" overflow="wrap" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const erc20Abi = ['function balanceOf(address) view returns (uint256)'];
const contract = new ethers.Contract('0xContractAddress', erc20Abi, provider);

const balance = await contract.balanceOf('0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45');
console.log(ethers.formatEther(balance));
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { somnia } from 'viem/chains';

const client = createPublicClient({
  chain: somnia,
  transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const data = await client.call({
  to: '0xContractAddress',
  data: '0x70a08231000000000000000000000000742d35cc6634c0532925a3b844bc9e7595f8bb45'
});
console.log(data);
```
{% endcode %}
{% endtab %}
{% endtabs %}
