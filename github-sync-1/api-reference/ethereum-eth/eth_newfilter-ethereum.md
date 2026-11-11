---
description: >-
  Example code for the eth_newFilter JSON RPC method. Сomplete guide on how to
  use eth_newFilter JSON RPC in GetBlock Web3 documentation.
---

# eth\_newFilter - Ethereum

This method creates a log filter that matches events emitted by contracts based on address, topic, and block-range criteria. It returns a filter ID that subsequent `eth_getFilterChanges` or `eth_getFilterLogs` calls use to fetch matching logs. Filters are stateful — they persist on the node until explicitly uninstalled or expired.

## Parameters

| Parameter | Type   | Required | Description                 |
| --------- | ------ | -------- | --------------------------- |
| `filter`  | object | Yes      | Filter criteria (see below) |

### Filter Object

| Field       | Type                      | Required | Description                                                                                               |
| ----------- | ------------------------- | -------- | --------------------------------------------------------------------------------------------------------- |
| `fromBlock` | string                    | No       | Starting block — hex number or `latest`, `earliest`, `pending`, `finalized`, `safe`. Defaults to `latest` |
| `toBlock`   | string                    | No       | Ending block — hex number or tag. Defaults to `latest`                                                    |
| `address`   | string or array of string | No       | Contract address(es) to filter logs from                                                                  |
| `topics`    | array                     | No       | Topic filters — up to 4 topics; each can be a single value, array (OR), or `null` (any)                   |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_newFilter",
    "params": [
        {
            "fromBlock": "0x1720340",
            "toBlock": "latest",
            "address": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"
            ]
        }
    ],
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
    method: 'eth_newFilter',
    params: [
        {
            "fromBlock": "0x1720340",
            "toBlock": "latest",
            "address": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"
            ]
        }
    ],
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
        'method': 'eth_newFilter',
        'params': [
        {
            "fromBlock": "0x1720340",
            "toBlock": "latest",
            "address": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"
            ]
        }
    ],
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
            "method": "eth_newFilter",
            "params": [
        {
            "fromBlock": "0x1720340",
            "toBlock": "latest",
            "address": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"
            ]
        }
    ],
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
    "result": "0x16"
}
```

## Response Parameters

| Parameter | Type   | Description                                                                   |
| --------- | ------ | ----------------------------------------------------------------------------- |
| `jsonrpc` | string | JSON-RPC protocol version ("2.0")                                             |
| `id`      | string | Request identifier matching the request                                       |
| `result`  | string | Hex-encoded filter ID — pass to `eth_getFilterChanges` or `eth_getFilterLogs` |

## Use Cases

* **ERC-20 Transfer Monitoring**: Filter for Transfer events emitted by a token contract to build a wallet activity feed
* **DEX Trade Indexing**: Filter for Swap events on Uniswap V3 pools to feed an indexer
* **Governance Vote Tracking**: Filter for governance events (`VoteCast`, `ProposalCreated`) across DAO contracts
* **Multi-Contract Aggregation**: Pass an array of addresses to filter events from multiple contracts in a single filter

## Error Handling

| Error Code | Message        | Description                                          |
| ---------- | -------------- | ---------------------------------------------------- |
| -32602     | Invalid params | Filter object is malformed or block range is invalid |
| -32603     | Internal error | Node failed to create the filter                     |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const filter = {
    address: '0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2',
    topics: ['0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef'],
};
provider.on(filter, (log) => {
    console.log('Transfer log:', log);
});

const filterId = await provider.send('eth_newFilter', [{
    fromBlock: 'latest',
    address: '0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2',
    topics: ['0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef'],
}]);
console.log('Filter ID:', filterId);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { mainnet } from 'viem/chains';

const client = createPublicClient({
    chain: mainnet,
    transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/'),
});

const filter = await client.createEventFilter({
    address: '0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2',
    event: {
        type: 'event',
        name: 'Transfer',
        inputs: [
            { indexed: true, name: 'from', type: 'address' },
            { indexed: true, name: 'to', type: 'address' },
            { indexed: false, name: 'value', type: 'uint256' },
        ],
    },
});
console.log('Filter ID:', filter.id);
```
{% endcode %}
{% endtab %}
{% endtabs %}
