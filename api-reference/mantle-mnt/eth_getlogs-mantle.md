---
description: >-
  Example code for the eth_getLogs JSON-RPC method. Complete guide on how to use
  eth_getLogs JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getLogs - Mantle

This method returns an array of all logs matching a given filter object on the Mantle network.

### Parameters

| Parameter    | Type   | Description        |
| ------------ | ------ | ------------------ |
| filterObject | object | The filter options |

Filter Object:

| Field     | Type         | Description                                           |
| --------- | ------------ | ----------------------------------------------------- |
| fromBlock | string       | (optional) Start block number or "earliest", "latest" |
| toBlock   | string       | (optional) End block number or "earliest", "latest"   |
| address   | string/array | (optional) Contract address or list of addresses      |
| topics    | array        | (optional) Array of topic filters                     |
| blockHash | string       | (optional) Specific block hash to filter              |

### Request examples

{% tabs %}
{% tab title="curl" %}
{% code title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getLogs",
    "params": [{
        "fromBlock": "0x3F6700",
        "toBlock": "0x3F6800",
        "address": "0x201EBa5CC46D216Ce6DC03F6a759e8E766e956aE",
        "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]
    }],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (axios)" %}
{% code title="example.js" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_getLogs",
    "params": [{
        "fromBlock": "0x3F6700",
        "toBlock": "0x3F6800",
        "address": "0x201EBa5CC46D216Ce6DC03F6a759e8E766e956aE",
        "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]
    }],
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
{% code title="example.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_getLogs",
    "params": [{
        "fromBlock": "0x3F6700",
        "toBlock": "0x3F6800",
        "address": "0x201EBa5CC46D216Ce6DC03F6a759e8E766e956aE",
        "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]
    }],
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
            "method": "eth_getLogs",
            "params": [{
                "fromBlock": "0x3F6700",
                "toBlock": "0x3F6800",
                "address": "0x201EBa5CC46D216Ce6DC03F6a759e8E766e956aE",
                "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]
            }],
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

{% code title="response.json" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": [
        {
            "address": "0x201EBa5CC46D216Ce6DC03F6a759e8E766e956aE",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                "0x000000000000000000000000...",
                "0x000000000000000000000000..."
            ],
            "data": "0x0000000000000000000000000000000000000000000000000de0b6b3a7640000",
            "blockNumber": "0x3F6750",
            "transactionHash": "0x...",
            "transactionIndex": "0x0",
            "blockHash": "0x...",
            "logIndex": "0x0",
            "removed": false
        }
    ]
}
```
{% endcode %}

### Response Parameters

| Field           | Type    | Description                           |
| --------------- | ------- | ------------------------------------- |
| address         | string  | Contract address that emitted the log |
| topics          | array   | Array of indexed log topics           |
| data            | string  | Non-indexed log data                  |
| blockNumber     | string  | Block number (hex)                    |
| transactionHash | string  | Transaction hash                      |
| logIndex        | string  | Log index in the block                |
| removed         | boolean | True if log was removed due to reorg  |

### Use Case

The `eth_getLogs` method is essential for:

* Event monitoring and indexing
* ERC-20 Transfer event tracking
* DeFi protocol event analysis
* NFT activity monitoring
* Smart contract event listening
* Historical data extraction

### Error Handling

| Status Code | Error Message        | Cause                           |
| ----------- | -------------------- | ------------------------------- |
| 403         | Forbidden            | Missing or invalid ACCESS-TOKEN |
| -32602      | Invalid params       | Invalid filter parameters       |
| -32005      | Query limit exceeded | Block range too large           |

### Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const logs = await provider.getLogs({
    address: '0x201EBa5CC46D216Ce6DC03F6a759e8E766e956aE',
    fromBlock: 0x3F6700,
    toBlock: 0x3F6800,
    topics: ['0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef']
});
console.log('Logs:', logs);
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

const logs = await client.getLogs({
    address: '0x201EBa5CC46D216Ce6DC03F6a759e8E766e956aE',
    fromBlock: 0x3F6700n,
    toBlock: 0x3F6800n,
    events: ['0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef']
});
console.log('Logs:', logs);
```
{% endcode %}
{% endtab %}
{% endtabs %}
