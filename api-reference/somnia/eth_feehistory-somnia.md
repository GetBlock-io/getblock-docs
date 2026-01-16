---
description: >-
  Example code for the eth_feeHistory JSON-RPC method. Complete guide on how to
  use eth_feeHistory JSON-RPC in GetBlock Web3 documentation.
---

# eth\_feeHistory - Somnia

This method returns historical gas information on the Somnia network. This is useful for estimating appropriate gas prices based on recent network activity. Somnia's predictable gas pricing through IceDB makes this data particularly reliable.

## Parameters

| Parameter         | Type   | Required | Description                                  |
| ----------------- | ------ | -------- | -------------------------------------------- |
| blockCount        | string | Yes      | Number of blocks to return (hex)             |
| newestBlock       | string | Yes      | Highest block number, or "latest", "pending" |
| rewardPercentiles | array  | No       | Percentiles for priority fee sampling        |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="curl" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "eth_feeHistory",
  "params": ["0x5", "latest", [25, 50, 75]]
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'eth_feeHistory',
  params: ['0x5', 'latest', [25, 50, 75]]
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
{% code title="example.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "eth_feeHistory",
    "params": ["0x5", "latest", [25, 50, 75]]
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
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
        "id": "getblock.io",
        "method": "eth_feeHistory",
        "params": ["0x5", "latest", [25, 50, 75]]
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

{% code title="response.json" overflow="wrap" %}
```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": {
    "baseFeePerGas": ["0x3b9aca00", "0x3b9aca00", "0x3b9aca00", "0x3b9aca00", "0x3b9aca00", "0x3b9aca00"],
    "gasUsedRatio": [0.5, 0.3, 0.4, 0.6, 0.2],
    "oldestBlock": "0x1a2b38",
    "reward": [
      ["0x0", "0x0", "0x0"],
      ["0x0", "0x0", "0x0"],
      ["0x0", "0x0", "0x0"],
      ["0x0", "0x0", "0x0"],
      ["0x0", "0x0", "0x0"]
    ]
  }
}
```
{% endcode %}

## Response Parameters

| Parameter     | Type   | Description                           |
| ------------- | ------ | ------------------------------------- |
| baseFeePerGas | array  | Base fee for each block (n+1 entries) |
| gasUsedRatio  | array  | Gas utilization ratio (0.0 to 1.0)    |
| oldestBlock   | string | First block in the range              |
| reward        | array  | Priority fee at each percentile       |

## Use Cases

* Implement dynamic gas pricing
* Analyze network congestion patterns
* Optimize transaction timing
* Build gas estimation algorithms
* Monitor network utilization

## Error Handling

| Error Code | Description                                   |
| ---------- | --------------------------------------------- |
| -32602     | Invalid params - invalid block count or range |
| -32603     | Internal error - node processing issues       |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const feeData = await provider.getFeeData();
console.log(feeData);
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

const feeHistory = await client.getFeeHistory({
  blockCount: 5,
  rewardPercentiles: [25, 50, 75]
});
console.log(feeHistory);
```
{% endcode %}
{% endtab %}
{% endtabs %}
