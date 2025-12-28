---
description: >-
  Example code for the eth_getLogs JSON RPC method. Сomplete guide on how to use
  eth_getLogsGetBlock JSON RPC in GetBlock Web3 documentation.
---

# eth\_getLogs - HyperEVM

This method returns an array of all logs matching a given filter object.

## Parameters

| Parameter    | Type   | Required | Description            |
| ------------ | ------ | -------- | ---------------------- |
| filterObject | object | Yes      | Filter options object. |

### Filter Object

| Field     | Type         | Required | Description                                             |
| --------- | ------------ | -------- | ------------------------------------------------------- |
| fromBlock | string       | No       | Start block (hex) or "latest".                          |
| toBlock   | string       | No       | End block (hex) or "latest".                            |
| address   | string/array | No       | Contract address or array of addresses.                 |
| topics    | array        | No       | Array of topic filters (max 4 topics).                  |
| blockHash | string       | No       | Specific block hash (alternative to fromBlock/toBlock). |

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
        "toBlock": "0x32",
        "address": "0xContractAddress",
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
        "toBlock": "0x32",
        "address": "0xContractAddress",
        "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]
    }],
    "id": "getblock.io"
};

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
        "toBlock": "0x32",
        "address": "0xContractAddress",
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
                "toBlock": "0x32",
                "address": "0xContractAddress",
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

{% code title="response.json" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": [
        {
            "address": "0xContractAddress",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                "0x000000000000000000000000sender",
                "0x000000000000000000000000recipient"
            ],
            "data": "0x0000000000000000000000000000000000000000000000000de0b6b3a7640000",
            "blockNumber": "0x1b4",
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

## Response Parameters

| Field           | Type    | Description                            |
| --------------- | ------- | -------------------------------------- |
| address         | string  | Contract address that emitted the log. |
| topics          | array   | Indexed event parameters.              |
| data            | string  | Non-indexed event data (hex).          |
| blockNumber     | string  | Block number (hex).                    |
| transactionHash | string  | Transaction hash.                      |
| logIndex        | string  | Log index in block (hex).              |
| removed         | boolean | True if log was removed due to reorg.  |

## Use Case

The `eth_getLogs` method is essential for:

* Event monitoring and indexing
* Token transfer tracking (ERC-20 Transfer events)
* DeFi protocol event processing
* Historical event analysis
* Building event-driven applications

## Error Handling

| Error Code | Message        | Cause                  |
| ---------- | -------------- | ---------------------- |
| -32602     | Invalid params | Invalid filter object. |
| -32603     | Internal error | Query range too large. |

Web3 Integration

{% tabs %}
{% tab title="Ether.js" %}
```javascript
import ethers from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

async function getLogs() {
    const logs = await provider.getLogs({
        fromBlock: 0x1,
        toBlock: 'latest',
        address: '0xContractAddress',
        topics: ['0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef']
    });
    console.log('Logs:', logs.length);
}

getLogs();
```
{% endtab %}

{% tab title="Viem" %}
{% code overflow="wrap" %}
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
    method: "eth_getLogs",
    params: [{
        "fromBlock": "0x1",
        "toBlock": "0x32",
        "address": "0xContractAddress",
        "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]
    }],
    });
    return result;
  } catch (error) {
    console.error("Viem Error:", error);
    throw error;
  }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
