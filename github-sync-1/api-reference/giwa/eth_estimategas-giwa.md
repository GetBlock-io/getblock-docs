---
description: >-
  Example code for the eth_estimateGas JSON-RPC method. Complete guide on how to
  use eth_estimateGas JSON-RPC in GetBlock Web3 documentation.
---

# eth\_estimateGas - GIWA

This method returns an estimate of the gas required to execute a transaction, without submitting it. On GIWA the estimate covers L2 execution gas; the OP Stack L1 data fee is separate.

## Parameters

| Parameter | Type | Required | Description |
| --------- | ---- | -------- | ----------- |
| transaction | object | Yes | Transaction call object |
| blockParameter | string | No | Block number in hex, or "latest", "pending" |

### Transaction Object

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| from | string | No | 20-byte sender address |
| to | string | No | 20-byte recipient/contract address (omit for deployment) |
| value | string | No | Value to send in wei (hex) |
| data | string | No | Encoded function call or deployment bytecode |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}

```bash
curl --location --request POST 'https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_estimateGas",
    "params": [{ "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "to": "0x4200000000000000000000000000000000000006", "value": "0xde0b6b3a7640000" }],
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
    method: 'eth_estimateGas',
    params: [{ from: '0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045', to: '0x4200000000000000000000000000000000000006', value: '0xde0b6b3a7640000' }],
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
        'method': 'eth_estimateGas',
        'params': [{ 'from': '0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045', 'to': '0x4200000000000000000000000000000000000006', 'value': '0xde0b6b3a7640000' }],
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
            "method": "eth_estimateGas",
            "params": [{ "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "to": "0x4200000000000000000000000000000000000006", "value": "0xde0b6b3a7640000" }],
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
    "result": "0x5208"
}
```

## Response Parameters

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| jsonrpc | string | JSON-RPC protocol version ("2.0") |
| id | string | Request identifier matching the request |
| result | string | Hex-encoded estimated L2 gas units |

## Use Cases

* **Gas Limits**: Set a safe gas limit before signing a transaction
* **Fee Preview**: Multiply the estimate by gas price to preview execution cost
* **Revert Detection**: Catch a would-fail transaction before broadcast
* **Deployment Sizing**: Estimate gas for contract creation calldata

## Error Handling

| Error Code | Message | Description |
| ---------- | ------- | ----------- |
| -32602 | Invalid params | Malformed transaction object |
| 3 | Execution reverted | The transaction would revert during estimation |
| -32000 | Execution error | Insufficient funds or gas required exceeds allowance |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}

```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/');

const gas = await provider.estimateGas({ from: '0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045', to: '0x4200000000000000000000000000000000000006', value: ethers.parseEther('1') });
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

const gas = await client.estimateGas({ account: '0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045', to: '0x4200000000000000000000000000000000000006', value: parseEther('1') });
```

{% endcode %}
{% endtab %}
{% endtabs %}
