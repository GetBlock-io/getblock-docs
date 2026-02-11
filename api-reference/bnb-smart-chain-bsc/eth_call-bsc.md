---
description: >-
  Example code for the eth_call JSON RPC method. Сomplete guide on how to use
  eth_call JSON RPC in GetBlock Web3 documentation.
---

# eth\_call - BSC

This method executes a new message call immediately without creating a transaction on the blockchain. This is commonly used for reading data from smart contracts, simulating transactions, and querying contract state on the BNB Smart Chain without spending gas.

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

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_call",
    "params": [{
        "to": "0x55d398326f99059fF775485246999027B3197955",
        "data": "0x70a08231000000000000000000000000742d35cc6634c0532925a3b844bc9e7595f8bb45"
    }, "latest"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'eth_call',
    params: [{
        to: '0x55d398326f99059fF775485246999027B3197955', // USDT on BSC
        data: '0x70a08231000000000000000000000000742d35cc6634c0532925a3b844bc9e7595f8bb45' // balanceOf
    }, 'latest'],
    id: 'getblock.io'
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
    "method": "eth_call",
    "params": [{
        "to": "0x55d398326f99059fF775485246999027B3197955",
        "data": "0x70a08231000000000000000000000000742d35cc6634c0532925a3b844bc9e7595f8bb45"
    }, "latest"],
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
        "method": "eth_call",
        "params": [{
            "to": "0x55d398326f99059fF775485246999027B3197955",
            "data": "0x70a08231000000000000000000000000742d35cc6634c0532925a3b844bc9e7595f8bb45"
        }, "latest"],
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
    "result": "0x0000000000000000000000000000000000000000000000000de0b6b3a7640000"
}
```

## Response Parameters

| Parameter | Type   | Description                           |
| --------- | ------ | ------------------------------------- |
| jsonrpc   | string | JSON-RPC version (2.0)                |
| id        | string | Request identifier                    |
| result    | string | Hex-encoded return data from the call |

## Use Cases

* Read BEP-20 token balances
* Query PancakeSwap pool reserves
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
{% code overflow="wrap" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const usdtAbi = ['function balanceOf(address) view returns (uint256)'];
const usdtContract = new ethers.Contract(
    '0x55d398326f99059fF775485246999027B3197955',
    usdtAbi,
    provider
);

const balance = await usdtContract.balanceOf('0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45');
console.log(ethers.formatUnits(balance, 18));
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http, parseAbi } from 'viem';
import { bsc } from 'viem/chains';

const client = createPublicClient({
    chain: bsc,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const balance = await client.readContract({
    address: '0x55d398326f99059fF775485246999027B3197955',
    abi: parseAbi(['function balanceOf(address) view returns (uint256)']),
    functionName: 'balanceOf',
    args: ['0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45']
});
console.log(balance);
```
{% endtab %}
{% endtabs %}
