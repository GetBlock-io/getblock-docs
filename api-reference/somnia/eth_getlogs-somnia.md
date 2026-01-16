---
description: >-
  Example code for the eth_getLogs JSON-RPC method. Complete guide on how to use
  eth_getLogs JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getLogs - Somnia

This method returns an array of all logs matching a given filter object on the Somnia network. This is essential for tracking events emitted by smart contracts, monitoring DeFi activities, and building event-driven applications.

## Parameters

| Parameter    | Type   | Required | Description        |
| ------------ | ------ | -------- | ------------------ |
| filterObject | object | Yes      | The filter options |

### Filter Object

| Field     | Type         | Required | Description                                  |
| --------- | ------------ | -------- | -------------------------------------------- |
| fromBlock | string       | No       | Starting block (hex or "latest", "earliest") |
| toBlock   | string       | No       | Ending block (hex or "latest", "earliest")   |
| address   | string/array | No       | Contract address(es) to filter               |
| topics    | array        | No       | Array of topic filters                       |
| blockHash | string       | No       | Restrict to single block by hash             |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "eth_getLogs",
  "params": [
    {
      "fromBlock": "0x1",
      "toBlock": "latest",
      "address": "0xContractAddress",
      "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]
    }
  ]
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code overflow="wrap" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "eth_getLogs",
  "params": [
    {
      "fromBlock": "0x1",
      "toBlock": "latest",
      "address": "0xContractAddress",
      "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]
    }
  ]
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers: {
        'Content-Type': 'application/json'
    },
    data: data
};

axios(config)
    .then(response => console.log(JSON.stringify(response.data)))
    .catch(error => console.log(error));
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code overflow="wrap" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "eth_getLogs",
  "params": [
    {
      "fromBlock": "0x1",
      "toBlock": "latest",
      "address": "0xContractAddress",
      "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]
    }
  ]
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code overflow="wrap" %}
```rust
use reqwest::header;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = reqwest::Client::new();
    
    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header(header::CONTENT_TYPE, "application/json")
        .body(r#"{
            "jsonrpc": "2.0",
            "id": "getblock.io",
            "method": "eth_getLogs",
            "params": [
              {
                "fromBlock": "0x1",
                "toBlock": "latest",
                "address": "0xContractAddress",
                "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]
              }
            ]
          }"#)
        .send()
        .await?;
    
    println!("{}", response.text().await?);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code title="Response (JSON)" overflow="wrap" %}
```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": [
    {
      "address": "0xContractAddress",
      "topics": [
        "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
        "0x000000000000000000000000742d35cc6634c0532925a3b844bc9e7595f8bb45",
        "0x0000000000000000000000008626f6940e2eb28930efb4cef49b2d1f2c9c1199"
      ],
      "data": "0x0000000000000000000000000000000000000000000000000de0b6b3a7640000",
      "blockNumber": "0x1a2b3c",
      "transactionHash": "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b",
      "transactionIndex": "0x0",
      "blockHash": "0x4e3a3754410177e6937ef1f84bba68ea139e8d1a2258c5f85db9f1cd715a1bdd",
      "logIndex": "0x0",
      "removed": false
    }
  ]
}
```
{% endcode %}

## Response Parameters

| Parameter       | Type    | Description                          |
| --------------- | ------- | ------------------------------------ |
| address         | string  | Contract that emitted the log        |
| topics          | array   | Indexed event parameters             |
| data            | string  | Non-indexed event data               |
| blockNumber     | string  | Block containing the log             |
| transactionHash | string  | Transaction that created the log     |
| logIndex        | string  | Log position in block                |
| removed         | boolean | True if log was removed due to reorg |

## Use Cases

* Track ERC-20 token transfers
* Monitor NFT mints and transfers
* Index DeFi protocol events
* Build event-driven applications
* Create analytics dashboards

## Error Handling

| Error Code | Description                             |
| ---------- | --------------------------------------- |
| -32602     | Invalid params - malformed filter       |
| -32603     | Internal error - node processing issues |
| -32005     | Query returned too many results         |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const logs = await provider.getLogs({
  fromBlock: 0,
  toBlock: 'latest',
  address: '0xContractAddress',
  topics: ['0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef']
});
console.log(logs);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { somnia } from 'viem/chains';

const client = createPublicClient({
  chain: somnia,
  transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const logs = await client.getLogs({
  address: '0xContractAddress',
  event: {
    name: 'Transfer',
    inputs: [
      { type: 'address', indexed: true, name: 'from' },
      { type: 'address', indexed: true, name: 'to' },
      { type: 'uint256', indexed: false, name: 'value' }
    ]
  },
  fromBlock: 0n,
  toBlock: 'latest'
});
console.log(logs);
```
{% endcode %}
{% endtab %}
{% endtabs %}
