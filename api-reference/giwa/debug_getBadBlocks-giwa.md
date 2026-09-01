---
description: >-
  Example code for the debug_getBadBlocks JSON-RPC method. Complete guide on how
  to use debug_getBadBlocks JSON-RPC in GetBlock Web3 documentation.
---

# debug\_getBadBlocks - GIWA

This method returns blocks the client recently rejected as invalid, with the reason. It is a Dedicated Node tier diagnostic used to investigate consensus or propagation issues.

{% hint style="warning" %}
This method belongs to the `debug` namespace and is available on GetBlock **Dedicated Nodes** only. It is not served on shared endpoints.
{% endhint %}

## Parameters

{% hint style="info" %}
This method does not require any parameters. Send the request with an empty `params` array.
{% endhint %}

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}

```bash
curl --location --request POST 'https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "debug_getBadBlocks",
    "params": [],
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
    method: 'debug_getBadBlocks',
    params: [],
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
        'method': 'debug_getBadBlocks',
        'params': [],
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
            "method": "debug_getBadBlocks",
            "params": [],
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
    "result": []
}
```

## Response Parameters

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| jsonrpc | string | JSON-RPC protocol version ("2.0") |
| id | string | Request identifier matching the request |
| result | array | Array of rejected block records (empty if none) |

## Use Cases

* **Consensus Debugging**: Inspect blocks the node rejected and why
* **Incident Analysis**: Correlate bad blocks with a network event
* **Client Diagnostics**: Detect propagation of malformed blocks
* **Monitoring**: Alert when bad blocks appear on an endpoint

## Error Handling

| Error Code | Message | Description |
| ---------- | ------- | ----------- |
| -32603 | Internal error | The node failed to return bad block records |
| -32601 | Method not found | The debug namespace is not enabled on this endpoint |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}

```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/');

const bad = await provider.send('debug_getBadBlocks', []);
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

const bad = await client.request({ method: 'debug_getBadBlocks' });
```

{% endcode %}
{% endtab %}
{% endtabs %}
