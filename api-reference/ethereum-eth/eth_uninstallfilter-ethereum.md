---
description: >-
  Example code for the eth_uninstallFilter JSON RPC method. Сomplete guide on
  how to use eth_uninstallFilter JSON RPC in GetBlock Web3 documentation.
---

# eth\_uninstallFilter - Ethereum

This method removes a previously created filter and frees its server-side state. Nodes automatically expire filters that haven't been polled recently (typically after \~5 minutes of inactivity), but explicit uninstall is polite server citizenship and required for long-lived clients that create many filters.

## Parameters

| Parameter  | Type   | Required | Description                                                                                                              |
| ---------- | ------ | -------- | ------------------------------------------------------------------------------------------------------------------------ |
| `filterId` | string | Yes      | Hex-encoded filter ID returned by a previous `eth_newFilter`, `eth_newBlockFilter`, or `eth_newPendingTransactionFilter` |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_uninstallFilter",
    "params": [
        "0x16"
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
    method: 'eth_uninstallFilter',
    params: [
        "0x16"
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
        'method': 'eth_uninstallFilter',
        'params': [
        "0x16"
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
            "method": "eth_uninstallFilter",
            "params": [
        "0x16"
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
    "result": true
}
```

## Response Parameters

| Parameter | Type    | Description                                                                                                                             |
| --------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| `jsonrpc` | string  | JSON-RPC protocol version ("2.0")                                                                                                       |
| `id`      | string  | Request identifier matching the request                                                                                                 |
| `result`  | boolean | `true` if the filter was found and removed, `false` if the filter ID didn't correspond to an active filter (already expired or invalid) |

## Use Cases

* **Filter Cleanup**: Free server-side filter state when a client is done using it
* **Server-Citizenship**: Prevent unbounded filter growth on shared endpoints
* **Reset Filter State**: Uninstall and re-create a filter to reset its polling window

## Error Handling

| Error Code | Message        | Description                           |
| ---------- | -------------- | ------------------------------------- |
| -32602     | Invalid params | Filter ID is malformed                |
| -32603     | Internal error | Node failed while removing the filter |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const removed = await provider.send('eth_uninstallFilter', ['0x16']);
console.log('Filter removed:', removed);
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

const removed = await client.uninstallFilter({ filter });
console.log('Filter removed:', removed);
```
{% endcode %}
{% endtab %}
{% endtabs %}
