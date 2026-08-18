---
description: >-
  Example code for the eth_estimateGas JSON RPC method. Сomplete guide on how to
  use eth_estimateGas JSON RPC in GetBlock Web3 documentation.
---

# eth\_estimateGas - Ethereum

This method returns an estimate of the gas needed to execute a transaction against the current state. It's typically an overestimate — the node runs the transaction and returns the actual gas cost plus a small buffer. Wallets use this to set the `gas` field before signing and broadcasting.

## Parameters

| Parameter        | Type   | Required | Description                                                                                        |
| ---------------- | ------ | -------- | -------------------------------------------------------------------------------------------------- |
| `transaction`    | object | Yes      | Transaction call object — same shape as `eth_call` transaction object                              |
| `blockParameter` | string | No       | Block number in hex, or `latest`, `earliest`, `pending`, `finalized`, `safe`. Defaults to `latest` |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_estimateGas",
    "params": [
        {
            "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
            "to": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
            "value": "0xde0b6b3a7640000"
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
    method: 'eth_estimateGas',
    params: [
        {
            "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
            "to": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
            "value": "0xde0b6b3a7640000"
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
        'method': 'eth_estimateGas',
        'params': [
        {
            "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
            "to": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
            "value": "0xde0b6b3a7640000"
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
            "method": "eth_estimateGas",
            "params": [
        {
            "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
            "to": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
            "value": "0xde0b6b3a7640000"
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
    "result": "0x5208"
}
```

## Response Parameters

| Parameter | Type   | Description                                                                     |
| --------- | ------ | ------------------------------------------------------------------------------- |
| `jsonrpc` | string | JSON-RPC protocol version ("2.0")                                               |
| `id`      | string | Request identifier matching the request                                         |
| `result`  | string | Hex-encoded gas estimate (typically slightly higher than actual execution cost) |

## Use Cases

* **Pre-Send Gas Estimation**: Set the `gas` field on a transaction before signing to avoid out-of-gas failures
* **Transaction Cost Preview**: Show users the expected gas cost before they confirm a transaction
* **Contract Interaction Sizing**: Estimate gas for arbitrary contract calls to display a fee preview in a dApp UI
* **Revert Detection**: A failing gas estimate typically indicates the transaction would revert on-chain

## Error Handling

| Error Code | Message            | Description                                                                   |
| ---------- | ------------------ | ----------------------------------------------------------------------------- |
| -32602     | Invalid params     | Transaction object is malformed                                               |
| 3          | Execution reverted | The transaction would revert — decode the return data to see the reason       |
| -32000     | Execution error    | Node failed to estimate gas (usually indicates the transaction can't succeed) |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const gasEstimate = await provider.estimateGas({
    from: '0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045',
    to: '0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2',
    value: ethers.parseEther('1.0'),
});
console.log('Gas estimate:', gasEstimate.toString());
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http, parseEther } from 'viem';
import { mainnet } from 'viem/chains';

const client = createPublicClient({
    chain: mainnet,
    transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/'),
});

const gas = await client.estimateGas({
    account: '0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045',
    to: '0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2',
    value: parseEther('1.0'),
});
console.log('Gas estimate:', gas);
```
{% endcode %}
{% endtab %}
{% endtabs %}
