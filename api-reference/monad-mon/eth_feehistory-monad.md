---
description: >-
  Example code for the eth_feeHistory JSON-RPC method. Complete guide on how to
  use eth_feeHistory JSON-RPC in GetBlock Web3 documentation.
---

# eth\_feeHistory - Monad

This method returns historical gas information for a range of blocks, useful for EIP-1559 fee estimation.

## Parameters

| Parameter         | Type   | Required | Description                                             |
| ----------------- | ------ | -------- | ------------------------------------------------------- |
| blockCount        | string | Yes      | Number of blocks to return (hex), max 1024.             |
| newestBlock       | string | Yes      | Highest block number in hex, or "latest", "pending".    |
| rewardPercentiles | array  | No       | Array of percentiles (0-100) for priority fee sampling. |

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

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "oldestBlock": "0x1b0",
        "baseFeePerGas": [
            "0x7",
            "0x7",
            "0x7",
            "0x7",
            "0x7",
            "0x7"
        ],
        "gasUsedRatio": [
            0.25,
            0.30,
            0.28,
            0.35,
            0.22
        ],
        "reward": [
            ["0x3b9aca00", "0x3b9aca00", "0x3b9aca00"],
            ["0x3b9aca00", "0x3b9aca00", "0x3b9aca00"],
            ["0x3b9aca00", "0x3b9aca00", "0x3b9aca00"],
            ["0x3b9aca00", "0x3b9aca00", "0x3b9aca00"],
            ["0x3b9aca00", "0x3b9aca00", "0x3b9aca00"]
        ]
    }
}
```

## Response Parameters

| Field         | Type   | Description                                                        |
| ------------- | ------ | ------------------------------------------------------------------ |
| oldestBlock   | string | Lowest block number in the returned range (hex).                   |
| baseFeePerGas | array  | Array of base fees per gas for each block (includes next block).   |
| gasUsedRatio  | array  | Array of gas used ratios (0-1) for each block.                     |
| reward        | array  | 2D array of priority fees at requested percentiles for each block. |

## Use Case

The `eth_feeHistory` method is essential for:

* EIP-1559 fee estimation
* Dynamic gas pricing algorithms
* Fee prediction for wallets
* Gas price analytics
* Transaction prioritization
* Cost optimization

## Error Handling

| Status Code | Error Message      | Cause                               |
| ----------- | ------------------ | ----------------------------------- |
| 403         | Forbidden          | Missing or invalid ACCESS-TOKEN.    |
| -32602      | Invalid params     | Invalid block count or percentiles. |
| -32000      | Resource not found | Block not found.                    |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// Get fee data (uses feeHistory internally)
const feeData = await provider.getFeeData();
console.log('Base fee:', ethers.formatUnits(feeData.gasPrice, 'gwei'), 'Gwei');
console.log('Max fee:', ethers.formatUnits(feeData.maxFeePerGas, 'gwei'), 'Gwei');
console.log('Priority fee:', ethers.formatUnits(feeData.maxPriorityFeePerGas, 'gwei'), 'Gwei');

// Direct feeHistory call
const feeHistory = await provider.send('eth_feeHistory', ['0x5', 'latest', [25, 50, 75]]);
console.log('Base fees:', feeHistory.baseFeePerGas);
```
{% endtab %}

{% tab title="Viem" %}
<pre class="language-js"><code class="lang-js">import { createPublicClient, http } from "viem";
<strong>import { monad } from "viem/chains";
</strong>/ Create Viem client with GetBlock
onst client = createPublicClient({
 chain: monad,
 transport: http(
   import.meta.env.MONAD_ACCESS_TOKEN
 ),
);
/ Using the method through Viem
sync function Call() {
 try {
   // Method-specific Viem implementation
   const result = await client.request({
    "method": "eth_feeHistory",
        "params": ["0x5", "latest", [25, 50, 75]],
   });
   console.log("Result:", result);
   return result;
 } catch (error) {
   console.error("Viem Error:", error);
   throw error;
 }
}
</code></pre>
{% endtab %}
{% endtabs %}
