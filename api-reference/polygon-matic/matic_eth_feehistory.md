---
description: >-
  Example code for the eth_feeHistory json-rpc method. Сomplete guide on how to
  use eth_feeHistory json-rpc in GetBlock.io Web3 documentation.
---

# eth\_feeHistory - Polygon

The **eth\_feeHistory** method returns historical gas information, including base fees and priority fees, for a range of blocks. This is useful for implementing dynamic gas pricing strategies.

## Parameters

| Parameter         | Type   | Required | Description                                           |
| ----------------- | ------ | -------- | ----------------------------------------------------- |
| blockCount        | string | Yes      | Number of blocks in the requested range (hex)         |
| newestBlock       | string | Yes      | Most recent block, "latest", "earliest", or "pending" |
| rewardPercentiles | array  | No       | Array of percentiles for priority fee data            |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_feeHistory",
    "params": ["0x4", "latest", [25, 50, 75]],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'eth_feeHistory',
    params: ["0x4", "latest", [25, 50, 75]],
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

{% tab title="Request" %}
{% code title="Python (requests)" %}
```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "eth_feeHistory",
    "params": ["0x4", "latest", [25, 50, 75]],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
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
            "params": ["0x4", "latest", [25, 50, 75]],
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

{% code title="Example response" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "oldestBlock": "0x4e83784",
        "reward": [
            [
                "0x1bf08eb000",
                "0x1bf08eb000",
                "0x246fc4ef80"
            ],
            [
                "0x1b72900c80",
                "0x1bf08eb000",
                "0x20aabe8bab"
            ],
            [
                "0x1bf08eb000",
                "0x1bf08eb000",
                "0x3a7b00d1e3"
            ],
            [
                "0x1bf08eb000",
                "0x1bf08eb000",
                "0x246d85ade0"
            ]
        ],
        "baseFeePerGas": [
            "0x768466dc41",
            "0x75df7ac885",
            "0x7538f45344",
            "0x74e55fd206",
            "0x74a85a9cf8"
        ],
        "gasUsedRatio": [
            0.4238737089435626,
            0.4204278540940883,
            0.5341368373683213,
            0.5651741156509931
        ],
        "baseFeePerBlobGas": [
            "0x0",
            "0x0",
            "0x0",
            "0x0",
            "0x0"
        ],
        "blobGasUsedRatio": [
            0,
            0,
            0,
            0
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

## Use Case

The eth\_feeHistory method is useful for:

* **Gas estimation**
* **Fee prediction**
* **EIP-1559 transactions**
* **Network analysis**

## Error Handling

| Status Code | Error Message   | Cause                           |
| ----------- | --------------- | ------------------------------- |
| 403         | Forbidden       | Missing or invalid ACCESS-TOKEN |
| -32600      | Invalid Request | Malformed request body          |
| -32602      | Invalid params  | Invalid method parameters       |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const result = await provider.send('eth_feeHistory', ["0x4", "latest", [25, 50, 75]]);
console.log('Result:', result);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { polygon } from 'viem/chains';

const client = createPublicClient({
    chain: polygon,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const result = await client.request({
    method: 'eth_feeHistory',
    params: ["0x4", "latest", [25, 50, 75]]
});
console.log('Result:', result);
```
{% endcode %}
{% endtab %}
{% endtabs %}
