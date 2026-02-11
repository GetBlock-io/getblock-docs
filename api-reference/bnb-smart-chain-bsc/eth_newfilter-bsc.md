---
description: >-
  Example code for the eth_newFilter JSON RPC method. Сomplete guide on how to
  use eth_newFilter JSON RPC in GetBlock Web3 documentation.
---

# eth\_newFilter - BSC

This method creates a filter object for logs on the BNB Smart Chain, which can be polled with `eth_getFilterChanges`. This is useful for tracking BEP-20 transfers and DeFi events.

## Parameters

| Parameter    | Type   | Required | Description    |
| ------------ | ------ | -------- | -------------- |
| filterObject | object | Yes      | Filter options |

### Filter Object

| Field     | Type         | Required | Description          |
| --------- | ------------ | -------- | -------------------- |
| fromBlock | string       | No       | Starting block       |
| toBlock   | string       | No       | Ending block         |
| address   | string/array | No       | Contract address(es) |
| topics    | array        | No       | Event topics         |

## Returns

| Field  | Type   | Description |
| ------ | ------ | ----------- |
| result | string | Filter ID   |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_newFilter",
    "params": [{
        "fromBlock": "latest",
        "address": "0x55d398326f99059fF775485246999027B3197955",
        "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]
    }],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'eth_newFilter',
    params: [{
        fromBlock: 'latest',
        address: '0x55d398326f99059fF775485246999027B3197955',
        topics: ['0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef']
    }],
    id: 'getblock.io'
};

axios.post(url, payload, {
    headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log(response.data))
.catch(error => console.error(error));
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "eth_newFilter",
    "params": [{
        "fromBlock": "latest",
        "address": "0x55d398326f99059fF775485246999027B3197955",
        "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]
    }],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endtab %}

{% tab title="Rust" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let payload = json!({
        "jsonrpc": "2.0",
        "method": "eth_newFilter",
        "params": [{
            "fromBlock": "latest",
            "address": "0x55d398326f99059fF775485246999027B3197955"
        }],
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
{% endtab %}
{% endtabs %}

## Response Example

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x1"
}
```

## Response Parameters

| Parameter | Type   | Description           |
| --------- | ------ | --------------------- |
| result    | string | Filter ID for polling |

## Use Cases

* Track BEP-20 token transfers
* Monitor PancakeSwap events
* Build notification systems
* Index contract events

## Error Handling

| Error Code | Description    |
| ---------- | -------------- |
| -32602     | Invalid params |
| -32603     | Internal error |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const filter = {
    address: '0x55d398326f99059fF775485246999027B3197955',
    topics: ['0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef']
};

provider.on(filter, (log) => {
    console.log('Transfer event:', log);
});
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http, parseAbiItem } from 'viem';
import { bsc } from 'viem/chains';

const client = createPublicClient({
    chain: bsc,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const unwatch = client.watchEvent({
    address: '0x55d398326f99059fF775485246999027B3197955',
    event: parseAbiItem('event Transfer(address indexed from, address indexed to, uint256 value)'),
    onLogs: logs => console.log(logs)
});
```
{% endtab %}
{% endtabs %}
