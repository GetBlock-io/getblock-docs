---
description: >-
  Example code for the eth_feeHistory JSON-RPC method. Complete guide on how to
  use eth_feeHistory JSON-RPC in GetBlock Web3 documentation.
---

# eth\_feeHistory - Mantle

This method returns historical gas information, allowing you to track trends over time. It returns an array of block-based fees per gas and transaction effective priority fees per gas percentiles.

### Parameters

| Parameter         | Type    | Description                                          |
| ----------------- | ------- | ---------------------------------------------------- |
| blockCount        | integer | Number of blocks in the requested range (1-1024)     |
| newestBlock       | string  | Highest block of the requested range (hex or tag)    |
| rewardPercentiles | array   | Array of percentile values to sample from each block |

### Request

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_feeHistory",
    "params": [4, "latest", [25, 75]],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (axios)" %}
{% code title="request.js" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_feeHistory",
    "params": [4, "latest", [25, 75]],
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

{% tab title="Python (requests)" %}
{% code title="request.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_feeHistory",
    "params": [4, "latest", [25, 75]],
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

{% tab title="Rust (reqwest)" %}
{% code title="main.rs" %}
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
            "params": [4, "latest", [25, 75]],
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

### Response

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "oldestBlock": "0x5594107",
        "reward": [
            [
                "0x0",
                "0x0"
            ],
            [
                "0x0",
                "0x0"
            ],
            [
                "0x0",
                "0x0"
            ],
            [
                "0x0",
                "0x0"
            ]
        ],
        "baseFeePerGas": [
            "0x1312d00",
            "0x1312d00",
            "0x1312d00",
            "0x1312d00",
            "0x1312d00"
        ],
        "gasUsedRatio": [
            2.34445e-7,
            2.76385e-7,
            3.19945e-7,
            2.34445e-7
        ],
        "baseFeePerBlobGas": [
            "0x1",
            "0x1",
            "0x1",
            "0x1",
            "0x1"
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

### Response Parameters

| Field         | Type   | Description                                                |
| ------------- | ------ | ---------------------------------------------------------- |
| baseFeePerGas | array  | Array of block base fees per gas (includes next block)     |
| gasUsedRatio  | array  | Array of block gas used ratios                             |
| oldestBlock   | string | Lowest block number of the returned range (hex)            |
| reward        | array  | Array of effective priority fee per gas percentile samples |

### Use Case

The `eth_feeHistory` method is essential for:

* EIP-1559 fee estimation
* Gas price trend analysis
* Transaction fee optimization
* Historical gas data analysis
* Wallet fee suggestions

### Error Handling

| Status Code | Error Message  | Cause                              |
| ----------- | -------------- | ---------------------------------- |
| 403         | Forbidden      | Missing or invalid ACCESS-TOKEN    |
| -32602      | Invalid params | Invalid block count or percentiles |

### Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const feeHistory = await provider.send('eth_feeHistory', [4, 'latest', [25, 75]]);
console.log('Fee History:', feeHistory);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { mantle } from 'viem/chains';

const client = createPublicClient({
    chain: mantle,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const feeHistory = await client.getFeeHistory({
    blockCount: 4,
    rewardPercentiles: [25, 75]
});
console.log('Fee History:', feeHistory);
```
{% endcode %}
{% endtab %}
{% endtabs %}
