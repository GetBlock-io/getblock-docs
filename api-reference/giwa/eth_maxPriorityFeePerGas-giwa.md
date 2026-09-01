---
description: >-
  Example code for the eth_maxPriorityFeePerGas JSON-RPC method. Complete guide
  on how to use eth_maxPriorityFeePerGas JSON-RPC in GetBlock Web3
  documentation.
---

# eth\_maxPriorityFeePerGas - GIWA

This method returns a suggested max priority fee per gas (the miner/sequencer tip) in wei for EIP-1559 transactions. It is used to build type-2 transactions that confirm promptly on GIWA.

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
    "method": "eth_maxPriorityFeePerGas",
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
    method: 'eth_maxPriorityFeePerGas',
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
        'method': 'eth_maxPriorityFeePerGas',
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
            "method": "eth_maxPriorityFeePerGas",
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
    "result": "0xf4240"
}
```

## Response Parameters

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| jsonrpc | string | JSON-RPC protocol version ("2.0") |
| id | string | Request identifier matching the request |
| result | string | Hex-encoded suggested priority fee in wei |

## Use Cases

* **EIP-1559 Fees**: Set maxPriorityFeePerGas on type-2 transactions
* **Fee Strategy**: Combine with base fee to compute maxFeePerGas
* **Inclusion Speed**: Bid a higher tip when faster inclusion is needed
* **Wallet UX**: Populate a default priority fee in a signing UI

## Error Handling

| Error Code | Message | Description |
| ---------- | ------- | ----------- |
| -32603 | Internal error | The node failed to compute a priority fee suggestion |
| -32601 | Method not found | The method is unavailable on the client build |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}

```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/');

const feeData = await provider.getFeeData();
console.log(feeData.maxPriorityFeePerGas);
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

const tip = await client.estimateMaxPriorityFeePerGas();
```

{% endcode %}
{% endtab %}
{% endtabs %}
