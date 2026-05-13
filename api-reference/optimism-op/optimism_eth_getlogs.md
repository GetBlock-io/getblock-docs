---
description: >-
  Example code for the eth_getLogs json-rpc method. Сomplete guide on how to use
  eth_getLogs json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getLogs - Optimism

This method retrieves logs (events) emitted by smart contracts based on filter criteria including address, topics, and block range. It's essential for tracking contract events like token transfers, approvals, and other state changes. Large queries may be limited, so use specific filters and reasonable block ranges.

## Parameters

{% hint style="info" %}
One parameter is required:
{% endhint %}

| Parameter    | Type   | Description                                                                              | Required |
| ------------ | ------ | ---------------------------------------------------------------------------------------- | -------- |
| filterObject | Object | Filter object with fromBlock, toBlock, address, topics, and blockhash (optional) fields. | Yes      |

## Request Sample

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getLogs",
    "params": [{"fromBlock": "0x7A69B00", "toBlock": "0x7A69B2C", "address": "0x4200000000000000000000000000000000000006", "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]}],
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
{% code overflow="wrap" %}
```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "eth_getLogs",
    "params": [{"fromBlock": "0x7A69B00", "toBlock": "0x7A69B2C", "address": "0x4200000000000000000000000000000000000006", "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]}],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code overflow="wrap" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let payload = json!({
    "jsonrpc": "2.0",
    "method": "eth_getLogs",
    "params": [{"fromBlock": "0x7A69B00", "toBlock": "0x7A69B2C", "address": "0x4200000000000000000000000000000000000006", "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]}],
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

## Response

A successful response returns the following:

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": [
        {
            "address": "0x4200000000000000000000000000000000000006",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                "0x0000000000000000000000004dc22588ade05c40338a9d95a6da9dcee68bcd60",
                "0x000000000000000000000000955954d5ac0a61b0996cced9d43e2534b0d99f5e"
            ],
            "data": "0x00000000000000000000000000000000000000000000000048670bf1b3f56d8b",
            "blockNumber": "0x7a69b00",
            "transactionHash": "0x5968592c02646a499f3e854b2603ce430ce3825a3c6b6aa7eaa2bfc8670f384c",
            "transactionIndex": "0xa",
            "blockHash": "0xc79de49c9770a414e1d9ecfd814e0b90652e801944e23971ad73c8e331c98901",
            "blockTimestamp": "0x67410fb9",
            "logIndex": "0x1f",
            "removed": false
        },
]
}
```
{% endcode %}

## Response Parameters

| Parameter       | Type   | Description      |
| --------------- | ------ | ---------------- |
| address         | string | Contract address |
| topics          | array  | Event topics     |
| data            | string | Event data       |
| blockNumber     | string | Block number     |
| transactionHash | string | Transaction hash |

## Use Case

The eth\_getLogs method is commonly used for:

* **Event monitoring**
* **Token transfer tracking**
* **Contract activity analysis**
* **DApp event handling**
* **Historical data retrieval**

## Error Handling

| Error Code | Description                       |
| ---------- | --------------------------------- |
| -32602     | Invalid params - malformed filter |
| -32005     | Query returned too many results   |
| -32603     | Internal error                    |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const logs = await provider.getLogs({
            fromBlock: "0x7A69B00",
            toBlock: "0x7A69B2C",
            address: "0x4200000000000000000000000000000000000006",
            topics: [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"
            ]
});
console.log('Found logs:', logs.length);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { optimism } from 'viem/chains';

const client = createPublicClient({
    chain: optimism,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const logs = await client.getLogs(
            fromBlock: "0x7A69B00",
            toBlock: "0x7A69B2C",
            address: "0x4200000000000000000000000000000000000006",
            topics: [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"
            ]
      );
console.log('Found logs:', logs.length);
```
{% endcode %}
{% endtab %}
{% endtabs %}
