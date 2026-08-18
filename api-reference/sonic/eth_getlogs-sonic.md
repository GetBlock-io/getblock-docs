---
description: >-
  Example code for the eth_getLogs JSON-RPC method. Complete guide on how to use
  eth_getLogs JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getLogs - Sonic

This method returns event logs that match a filter over a range of blocks. It is the primary method for reading historical events such as ERC-20 transfers and contract events.

## Parameters

| Parameter | Type   | Required | Description                                |
| --------- | ------ | -------- | ------------------------------------------ |
| filter    | object | Yes      | Filter object selecting the logs to return |

### Filter Object

| Field     | Type            | Required | Description                                            |
| --------- | --------------- | -------- | ------------------------------------------------------ |
| fromBlock | string          | No       | Start block in hex, or "latest", "earliest"            |
| toBlock   | string          | No       | End block in hex, or "latest", "earliest"              |
| address   | string or array | No       | Contract address or addresses to filter by             |
| topics    | array           | No       | Event signature and indexed topics to match            |
| blockHash | string          | No       | Restrict to a single block by hash, instead of a range |

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
    "params": [{"fromBlock": "0x1e8400", "toBlock": "0x1e8480", "address": "0x039e2fB66102314Ce7b64Ce5Ce3E5183bc94aD38", "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]}],
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
    params: [{"fromBlock": "0x1e8400", "toBlock": "0x1e8480", "address": "0x039e2fB66102314Ce7b64Ce5Ce3E5183bc94aD38", "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]}],
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
        'params': [{"fromBlock": "0x1e8400", "toBlock": "0x1e8480", "address": "0x039e2fB66102314Ce7b64Ce5Ce3E5183bc94aD38", "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]}],
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
            "params": [{"fromBlock": "0x1e8400", "toBlock": "0x1e8480", "address": "0x039e2fB66102314Ce7b64Ce5Ce3E5183bc94aD38", "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]}],
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
            "address": "0x039e2fB66102314Ce7b64Ce5Ce3E5183bc94aD38",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                "0x000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045",
                "0x00000000000000000000000029219dd400f2bf60e5a23d13be72b486d4038894"
            ],
            "data": "0x0000000000000000000000000000000000000000000000008ac7230489e80000",
            "blockNumber": "0x1e8460",
            "transactionHash": "0x4a7b0c3d6e9f2a5b8c1d4e7f0a3b6c9d2e5f8a1b4c7d0e3f6a9b2c5d8e1f4a7b",
            "transactionIndex": "0x0",
            "blockHash": "0x8e3f5b2a9c1d4e7f0a6b3c8d5e2f9a1b4c7d0e3f6a9b2c5d8e1f4a7b0c3d6e9f",
            "logIndex": "0x0",
            "removed": false
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

### Log Object

| Field           | Type    | Description                                             |
| --------------- | ------- | ------------------------------------------------------- |
| address         | string  | Contract that emitted the log                           |
| topics          | array   | Indexed event topics, starting with the event signature |
| data            | string  | Non-indexed event data, hex-encoded                     |
| blockNumber     | string  | Block the log was emitted in                            |
| transactionHash | string  | Transaction that emitted the log                        |
| removed         | boolean | Whether the log was removed by a reorganization         |

## Use Cases

* **Transfer Tracking**: Read ERC-20 Transfer events for an address or token
* **Event Indexing**: Ingest contract events into an off-chain database
* **DeFi Monitoring**: Watch swap, mint, and burn events on pools
* **Notifications**: Trigger alerts on specific contract events
* **Analytics**: Aggregate historical event data over block ranges

## Error Handling

| Error Code | Message        | Description                                                  |
| ---------- | -------------- | ------------------------------------------------------------ |
| -32602     | Invalid params | The filter object is malformed or the block range is invalid |
| -32005     | Limit exceeded | The requested block range or result set is too large         |
| -32603     | Internal error | The node failed to read the logs                             |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const filter = {
    address: '0x039e2fB66102314Ce7b64Ce5Ce3E5183bc94aD38',
    topics: ['0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef'],
    fromBlock: 2000000, toBlock: 2000128
};
const logs = await provider.getLogs(filter);
console.log(logs);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { sonic } from 'viem/chains';

const client = createPublicClient({ chain: sonic, transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/') });

const logs = await client.getLogs({
    address: '0x039e2fB66102314Ce7b64Ce5Ce3E5183bc94aD38',
    event: { type: 'event', name: 'Transfer', inputs: [
        { type: 'address', indexed: true, name: 'from' },
        { type: 'address', indexed: true, name: 'to' },
        { type: 'uint256', indexed: false, name: 'value' } ] },
    fromBlock: 2000000n, toBlock: 2000128n
});
console.log(logs);
```
{% endcode %}
{% endtab %}
{% endtabs %}
