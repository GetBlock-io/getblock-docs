---
description: >-
  Example code for the eth_getLogs JSON-RPC method. Complete guide on how to use
  eth_getLogs JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getLogs - Cronos

This method returns an array of all logs matching a given filter object in a single call. It is the primary method for querying event logs from smart contracts, essential for tracking token transfers, DEX swaps, and other on-chain events.

## Parameters

| Parameter    | Type   | Required | Description            |
| ------------ | ------ | -------- | ---------------------- |
| filterObject | object | Yes      | Filter criteria object |

### Filter Object

| Field     | Type   | Required | Description                                                               |
| --------- | ------ | -------- | ------------------------------------------------------------------------- |
| fromBlock | string | No       | Start block in hex, or "latest"                                           |
| toBlock   | string | No       | End block in hex, or "latest"                                             |
| address   | string | array    | No                                                                        |
| topics    | array  | No       | Ordered topic filters (null matches any position)                         |
| blockHash | string | No       | Restrict to a single block by hash (mutually exclusive with from/toBlock) |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getLogs",
    "params": [{ "fromBlock": "0x1900000", "toBlock": "0x1900100", "address": "0x5C7F8A570d578ED84E63fdFA7b1eE72dEae1AE23", "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"] }],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_getLogs',
    params: [{ fromBlock: '0x1900000', toBlock: '0x1900100', address: '0x5C7F8A570d578ED84E63fdFA7b1eE72dEae1AE23', topics: ['0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef'] }],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests

response = requests.post(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'eth_getLogs',
        'params': [{ 'fromBlock': '0x1900000', 'toBlock': '0x1900100', 'address': '0x5C7F8A570d578ED84E63fdFA7b1eE72dEae1AE23', 'topics': ['0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef'] }],
        'id': 'getblock.io'
    }
)

print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="example.rs" %}
```rust
use reqwest::Client;
use serde_json::{json, Value};

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let response = client
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "eth_getLogs",
            "params": [{ "fromBlock": "0x1900000", "toBlock": "0x1900100", "address": "0x5C7F8A570d578ED84E63fdFA7b1eE72dEae1AE23", "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"] }],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;

    println!("Result: {}", response["result"]);
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
    "result": [
        {
            "address": "0x5C7F8A570d578ED84E63fdFA7b1eE72dEae1AE23",
            "blockNumber": "0x1900000",
            "transactionHash": "0x3c8a1f5b2d9e4076c1a8b3d5e7f9021436587a9cbdef012345678abcdef901234",
            "logIndex": "0x0",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"
            ],
            "data": "0x"
        }
    ]
}
```

## Response Parameters

| Parameter | Type   | Description                              |
| --------- | ------ | ---------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")        |
| id        | string | Request identifier matching the request  |
| result    | array  | Array of log objects matching the filter |

## Use Cases

* **Token Transfers**: Track ERC-20 Transfer events by topic and address
* **DEX Activity**: Index swap and liquidity events from AMM pools
* **Indexing**: Backfill contract events over a block range
* **Analytics**: Aggregate on-chain events for dashboards

## Error Handling

| Error Code | Message        | Description                                          |
| ---------- | -------------- | ---------------------------------------------------- |
| -32602     | Invalid params | Malformed filter object or block range               |
| -32005     | Limit exceeded | The requested block range or result set is too large |
| -32603     | Internal error | The node failed to assemble the logs                 |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const logs = await provider.getLogs({ address: '0x5C7F8A570d578ED84E63fdFA7b1eE72dEae1AE23', topics: ['0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef'], fromBlock: 26214400 });
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';

const client = createPublicClient({
  transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/')
});

const logs = await client.getLogs({ address: '0x5C7F8A570d578ED84E63fdFA7b1eE72dEae1AE23', fromBlock: 26214400n });
```
{% endcode %}
{% endtab %}
{% endtabs %}
