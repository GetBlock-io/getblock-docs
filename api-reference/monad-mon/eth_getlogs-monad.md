---
description: >-
  Example code for the eth_getLogs JSON-RPC method. Complete guide on how to use
  eth_getLogs JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getLogs - Monad

This method returns an array of all logs matching a given filter object.

## Parameters

| Parameter        | Type         | Required | Description                                                               |
| ---------------- | ------------ | -------- | ------------------------------------------------------------------------- |
| filter           | object       | Yes      | The filter options.                                                       |
| filter.fromBlock | string       | No       | Start block (hex or tag). Default: "latest".                              |
| filter.toBlock   | string       | No       | End block (hex or tag). Default: "latest".                                |
| filter.address   | string/array | No       | Contract address or list of addresses.                                    |
| filter.topics    | array        | No       | Array of 32-byte topic filters.                                           |
| filter.blockHash | string       | No       | Restricts logs to a single block (cannot be used with fromBlock/toBlock). |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getLogs",
    "params": [{
        "fromBlock": "0x1",
        "toBlock": "0xa",
        "address": "0xdAC17F958D2ee523a2206206994597C13D831ec7",
        "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]
    }],
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
    "method": "eth_getLogs",
    "params": [{
        "fromBlock": "0x1",
        "toBlock": "0xa",
        "address": "0xdAC17F958D2ee523a2206206994597C13D831ec7",
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

{% tab title="Request" %}
{% code overflow="wrap" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_getLogs",
    "params": [{
        "fromBlock": "0x1",
        "toBlock": "0xa",
        "address": "0xdAC17F958D2ee523a2206206994597C13D831ec7",
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
            "method": "eth_getLogs",
            "params": [{
                "fromBlock": "0x1",
                "toBlock": "0xa",
                "address": "0xdAC17F958D2ee523a2206206994597C13D831ec7",
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

## Response

{% code title="Response (example)" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": [
        {
            "address": "0xdac17f958d2ee523a2206206994597c13d831ec7",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                "0x000000000000000000000000742d35cc6634c0532925a3b844bc9e7595f0beb",
                "0x0000000000000000000000004bbeeb066ed09b7aed07bf39eee0460dfa261520"
            ],
            "data": "0x0000000000000000000000000000000000000000000000000000000005f5e100",
            "blockNumber": "0x5",
            "transactionHash": "0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331",
            "transactionIndex": "0x0",
            "blockHash": "0xdc0818cf78f21a8e70579cb46a43643f78291264dda342ae31049421c82d21ae",
            "logIndex": "0x0",
            "removed": false
        }
    ]
}
```
{% endcode %}

## Response Parameters

| Field            | Type    | Description                                          |
| ---------------- | ------- | ---------------------------------------------------- |
| address          | string  | Address from which log originated.                   |
| topics           | array   | Array of 0-4 indexed log topics.                     |
| data             | string  | Non-indexed log data.                                |
| blockNumber      | string  | Block number where log was in (hex).                 |
| transactionHash  | string  | Transaction hash.                                    |
| transactionIndex | string  | Transaction index in the block (hex).                |
| blockHash        | string  | Block hash.                                          |
| logIndex         | string  | Log index in the block (hex).                        |
| removed          | boolean | True if log was removed due to chain reorganization. |

## Use Case

The `eth_getLogs` method is essential for:

* Tracking token transfers (ERC-20, ERC-721)
* Monitoring DeFi protocol events
* Building event-driven applications
* Indexing blockchain data
* Transaction history analysis
* Real-time event notifications

## Error Handling

| Status Code | Error Message      | Cause                                      |
| ----------- | ------------------ | ------------------------------------------ |
| 403         | Forbidden          | Missing or invalid ACCESS-TOKEN.           |
| -32602      | Invalid params     | Invalid filter parameters.                 |
| -32005      | Limit exceeded     | Too many results or block range too large. |
| -32000      | Resource not found | Block not found.                           |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// ERC-20 Transfer event signature
const transferTopic = ethers.id('Transfer(address,address,uint256)');

const logs = await provider.getLogs({
    fromBlock: 1,
    toBlock: 10,
    address: tokenAddress,
    topics: [transferTopic]
});

logs.forEach(log => {
    console.log('Transfer in block:', log.blockNumber);
});
```
{% endtab %}

{% tab title="Viem" %}
```js
import { createPublicClient, http } from 'viem';
import { monad } from 'viem/chains';

// Create Viem client with GetBlock
const client = createPublicClient({
  chain: monad,
  transport: http("https://go.getblock.us/<ACCESS_TOKEN>"),
});

// Using the method through Viem
async function fetchAccountViem() {
    try {
        // Method-specific Viem implementation
        const result = await client.request({
           method: "eth_getLogs",
           params: [{
            "fromBlock": "0x1",
            "toBlock": "0xa",
            "address": "0xdAC17F958D2ee523a2206206994597C13D831ec7",
            "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]
        }],
        });
        console.log('Result:', result);
        return result;
    } catch (error) {
        console.error('Viem Error:', error);
        throw error;
    }
}
```
{% endtab %}
{% endtabs %}

### Block Range Limit

{% hint style="warning" %}
Important: Monad has a maximum block range of 1000 blocks for `eth_getLogs`.

Due to Monad's high throughput (much larger blocks than Ethereum), we recommend using small block ranges (1-10 blocks) for optimal performance.
{% endhint %}

{% code title="Example: recommended vs not recommended" %}
```javascript
// Recommended: Small block ranges
const logs = await provider.getLogs({
    fromBlock: latestBlock - 10,
    toBlock: latestBlock,
    address: contractAddress
});

// Not recommended: Large ranges may timeout
// fromBlock: 0, toBlock: 'latest' // May fail
```
{% endcode %}

### Performance Tips

{% stepper %}
{% step %}
### Use specific addresses

Always filter by contract address when possible.
{% endstep %}

{% step %}
### Use topics

Filter by event signatures to reduce results.
{% endstep %}

{% step %}
### Paginate

For large queries, break into smaller block ranges.
{% endstep %}

{% step %}
### Use blockHash

When querying a single block, use the `blockHash` parameter.
{% endstep %}
{% endstepper %}
