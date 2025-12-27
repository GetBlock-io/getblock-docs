---
description: >-
  Example code for the eth_feeHistory JSON RPC method. Сomplete guide on how to
  use eth_feeHistory GetBlock JSON RPC in GetBlock Web3 documentation.
---

# eth\_feeHistory - HyperEVM

This method returns historical gas information for EIP-1559 fee estimation.

## Parameters

| Parameter         | Type   | Required | Description                              |
| ----------------- | ------ | -------- | ---------------------------------------- |
| blockCount        | string | Yes      | Number of blocks to return (hex).        |
| newestBlock       | string | Yes      | Newest block ("latest" or block number). |
| rewardPercentiles | array  | No       | Percentiles for priority fee sampling.   |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_feeHistory",
    "params": ["0x5", "latest", [25, 50, 75]],
    "id": "getblock.io"
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
    "method": "eth_feeHistory",
    "params": ["0x5", "latest", [25, 50, 75]],
    "id": "getblock.io"
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
    "method": "eth_feeHistory",
    "params": ["0x5", "latest", [25, 50, 75]],
    "id": "getblock.io"
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
            "method": "eth_feeHistory",
            "params": ["0x5", "latest", [25, 50, 75]],
            "id": "getblock.io"
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

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_feeHistory",
    "params": ["0x5", "latest", [25, 50, 75]],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Ethers.js)" %}
{% code title="ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

async function getFeeHistory() {
    const feeHistory = await provider.send('eth_feeHistory', ['0x5', 'latest', [25, 50, 75]]);
    console.log('Base Fee Per Gas:', feeHistory.baseFeePerGas);
}

getFeeHistory();
```
{% endcode %}
{% endtab %}

{% tab title="Python (Web3.py)" %}
{% code title="web3.py" %}
```python
from web3 import Web3

w3 = Web3(Web3.HTTPProvider('https://go.getblock.io/<ACCESS-TOKEN>/'))

fee_history = w3.eth.fee_history(5, 'latest', [25, 50, 75])
print(f'Base Fee History: {fee_history["baseFeePerGas"]}')
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="Rust (reqwest)" %}
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
            "method": "eth_feeHistory",
            "params": ["0x5", "latest", [25, 50, 75]],
            "id": "getblock.io"
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

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "oldestBlock": "0x1b0",
        "baseFeePerGas": ["0x3b9aca00", "0x3b9aca00", "0x3b9aca00", "0x3b9aca00", "0x3b9aca00", "0x3b9aca00"],
        "gasUsedRatio": [0.0, 0.1, 0.05, 0.0, 0.2],
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

## Response Parameters

| Field         | Type   | Description                             |
| ------------- | ------ | --------------------------------------- |
| oldestBlock   | string | Oldest block in returned range (hex).   |
| baseFeePerGas | array  | Base fee per gas for each block (hex).  |
| gasUsedRatio  | array  | Gas used ratio (0 to 1) for each block. |
| reward        | array  | Priority fee percentiles per block.     |

## Use Case

The `eth_feeHistory` method is essential for:

* EIP-1559 fee estimation
* Dynamic gas price strategies
* Transaction cost prediction
* Fee optimization algorithms
* Network congestion analysis

## Error Handling

| Error Code | Message        | Cause                               |
| ---------- | -------------- | ----------------------------------- |
| -32602     | Invalid params | Invalid block count or percentiles. |
| -32603     | Internal error | Node issue.                         |

### Web3 Integration

{% tabs %}
{% tab title="Ether.js" %}
{% code overflow="wrap" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

async function getFeeHistory() {
    const feeHistory = await provider.send('eth_feeHistory', ['0x5', 'latest', [25, 50, 75]]);
    console.log('Base Fee Per Gas:', feeHistory.baseFeePerGas);
}

getFeeHistory();
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
```jsx
import { createPublicClient, http } from "viem";
import { hyperEvm } from "viem/chains";

// Create Viem client with GetBlock
const client = createPublicClient({
  chain: hyperEvm,
  transport: http(import.meta.env.HYPER_ACCESS_TOKEN),
});

// Using the method through Viem
async function Call() {
  try {
    // Method-specific Viem implementation
    const result = await client.request({
    method: "eth_feeHistory",
    params: ["0x5", "latest", [25, 50, 75]],
    });
    return result;
  } catch (error) {
    console.error("Viem Error:", error);
    throw error;
  }
}
```
{% endtab %}
{% endtabs %}
