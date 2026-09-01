---
description: >-
  Example code for the eth_getTransactionCount JSON-RPC method. Complete guide
  on how to use eth_getTransactionCount JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getTransactionCount - GIWA

This method returns the number of transactions sent from an address, as a hex-encoded integer. It provides the nonce used when building the next transaction.

## Parameters

| Parameter | Type | Required | Description |
| --------- | ---- | -------- | ----------- |
| address | string | Yes | 20-byte address to query |
| blockParameter | string | Yes | Block number in hex, or "latest", "earliest", "pending" |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}

```bash
curl --location --request POST 'https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getTransactionCount",
    "params": ["0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "latest"],
    "id": "getblock.io"
}'
```

{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}

```javascript
const axios = require('axios');

const response = await axios.post('https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_getTransactionCount',
    params: ['0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045', 'latest'],
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
    'https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'eth_getTransactionCount',
        'params': ['0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045', 'latest'],
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
        .post("https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "eth_getTransactionCount",
            "params": ["0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "latest"],
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
    "result": "0x2a"
}
```

## Response Parameters

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| jsonrpc | string | JSON-RPC protocol version ("2.0") |
| id | string | Request identifier matching the request |
| result | string | Hex-encoded transaction count (nonce) for the address |

## Use Cases

* **Nonce Management**: Assign the next nonce when signing transactions client-side
* **Pending Nonce**: Pass "pending" to include queued transactions when building the next one
* **Stuck Transaction Recovery**: Rebroadcast with the correct nonce to replace a pending transaction
* **Activity Metrics**: Measure how many transactions an address has sent

## Error Handling

| Error Code | Message | Description |
| ---------- | ------- | ----------- |
| -32602 | Invalid params | Malformed address or block parameter |
| -32603 | Internal error | The node failed to read the nonce |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}

```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/');

const nonce = await provider.getTransactionCount('0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045', 'pending');
```

{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}

```javascript
import { createPublicClient, http } from 'viem';

const client = createPublicClient({
  transport: http('https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/')
});

const nonce = await client.getTransactionCount({ address: '0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045' });
```

{% endcode %}
{% endtab %}
{% endtabs %}
