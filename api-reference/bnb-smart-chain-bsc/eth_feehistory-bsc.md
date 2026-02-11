---
description: >-
  Example code for the eth_feeHistory JSON RPC method. Сomplete guide on how to
  use eth_feeHistory JSON RPC in GetBlock Web3 documentation.
---

# eth\_feeHistory - BSC

This method returns historical gas information for fee estimation on BSC.

{% hint style="info" %}
Note: BSC uses legacy gas pricing and does not fully implement EIP-1559, so this method may have limited functionality compared to Ethereum.
{% endhint %}

## Parameters

| Parameter         | Type   | Required | Description                             |
| ----------------- | ------ | -------- | --------------------------------------- |
| blockCount        | string | Yes      | Number of blocks to return (hex)        |
| newestBlock       | string | Yes      | Newest block ("latest" or block number) |
| rewardPercentiles | array  | No       | Percentile values for priority fees     |

## Request Example

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

{% tab title="JavaScript (Axios)" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'eth_feeHistory',
    params: ['0x5', 'latest', [25, 50, 75]],
    id: 'getblock.io'
};

axios.post(url, payload, {
    headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log(JSON.stringify(response.data, null, 2)))
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
    "method": "eth_feeHistory",
    "params": ["0x5", "latest", [25, 50, 75]],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(json.dumps(response.json(), indent=2))
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
        "method": "eth_feeHistory",
        "params": ["0x5", "latest", [25, 50, 75]],
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
    "result": {
        "oldestBlock": "0x4a8f7e7",
        "reward": [
            [
                "0x2faf080",
                "0x3938700",
                "0x5f5e100"
            ]
        ],
        "baseFeePerGas": [
            "0x0",
            "0x0"
        ],
        "gasUsedRatio": [
            0.10401812727272727
        ],
        "baseFeePerBlobGas": [
            "0x1",
            "0x1"
        ],
        "blobGasUsedRatio": [
            0.16666666666666666
        ]
    }
}
```
{% endcode %}

## Response Parameters

| Parameter     | Type   | Description                    |
| ------------- | ------ | ------------------------------ |
| baseFeePerGas | array  | Base fees (typically 0 on BSC) |
| gasUsedRatio  | array  | Ratio of gas used per block    |
| oldestBlock   | string | Starting block number          |
| reward        | array  | Priority fee percentiles       |

## Use Cases

* Analyze historical gas usage patterns
* Monitor network congestion trends
* Build gas price prediction models
* Optimize transaction timing

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
console.log('Gas Price:', ethers.formatUnits(feeData.gasPrice, 'gwei'), 'Gwei');
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem.js" %}
```javascript
import { createPublicClient, http, formatGwei } from 'viem';
import { bsc } from 'viem/chains';

const client = createPublicClient({
    chain: bsc,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const gasPrice = await client.getGasPrice();
console.log('Gas Price:', formatGwei(gasPrice), 'Gwei');
```
{% endcode %}
{% endtab %}
{% endtabs %}
