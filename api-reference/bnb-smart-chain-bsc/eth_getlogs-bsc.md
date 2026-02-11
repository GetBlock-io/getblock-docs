---
description: >-
  Example code for the eth_getLogs JSON RPC method. Сomplete guide on how to use
  eth_getLogs JSON RPC in GetBlock Web3 documentation.
---

# eth\_getLogs - BSC

This method returns an array of all logs matching a given filter object on the BNB Smart Chain. This is essential for tracking events emitted by smart contracts, monitoring BEP-20 transfers, and building event-driven applications.

## Parameters

| Parameter    | Type   | Required | Description        |
| ------------ | ------ | -------- | ------------------ |
| filterObject | object | Yes      | The filter options |

### Filter Object

| Field     | Type         | Required | Description                      |
| --------- | ------------ | -------- | -------------------------------- |
| fromBlock | string       | No       | Starting block (hex or "latest") |
| toBlock   | string       | No       | Ending block (hex or "latest")   |
| address   | string/array | No       | Contract address(es)             |
| topics    | array        | No       | Event topic filters              |
| blockHash | string       | No       | Specific block hash              |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getLogs",
    "params": [{
        "fromBlock": "latest",
        "toBlock": "latest",
        "address": "0x55d398326f99059fF775485246999027B3197955",
        "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]
    }],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="request.js" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'eth_getLogs',
    params: [{
        fromBlock: 'latest',
        toBlock: 'latest',
        address: '0x55d398326f99059fF775485246999027B3197955',
        topics: ['0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef']
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
```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "eth_getLogs",
    "params": [{
        "fromBlock": "latest",
        "toBlock": "latest",
        "address": "0x55d398326f99059fF775485246999027B3197955",
        "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]
    }],
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
        "method": "eth_getLogs",
        "params": [{
            "fromBlock": "latest",
            "toBlock": "latest",
            "address": "0x55d398326f99059fF775485246999027B3197955"
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
{% endtab %}
{% endtabs %}

## Response Example

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": [{
        "address": "0x55d398326f99059ff775485246999027b3197955",
        "topics": [
            "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
            "0x000000000000000000000000abc123...",
            "0x000000000000000000000000def456..."
        ],
        "data": "0x00000000000000000000000000000000000000000000000000000000000f4240",
        "blockNumber": "0x2625a01",
        "transactionHash": "0x123...",
        "transactionIndex": "0x0",
        "blockHash": "0x456...",
        "logIndex": "0x0",
        "removed": false
    }]
}
```

## Response Parameters

| Parameter       | Type   | Description      |
| --------------- | ------ | ---------------- |
| address         | string | Contract address |
| topics          | array  | Event topics     |
| data            | string | Event data       |
| blockNumber     | string | Block number     |
| transactionHash | string | Transaction hash |

## Use Cases

* Track BEP-20 token transfers
* Monitor PancakeSwap swaps
* Index DeFi protocol events
* Build analytics dashboards

## Error Handling

| Error Code | Description                       |
| ---------- | --------------------------------- |
| -32602     | Invalid params - malformed filter |
| -32005     | Query returned too many results   |
| -32603     | Internal error                    |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const logs = await provider.getLogs({
    address: '0x55d398326f99059fF775485246999027B3197955',
    topics: ['0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef'],
    fromBlock: 40000000,
    toBlock: 'latest'
});
console.log('Found logs:', logs.length);
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

const logs = await client.getLogs({
    address: '0x55d398326f99059fF775485246999027B3197955',
    fromBlock: 40000000n,
    toBlock: 'latest'
});
console.log('Found logs:', logs.length);
```
{% endcode %}
{% endtab %}
{% endtabs %}
