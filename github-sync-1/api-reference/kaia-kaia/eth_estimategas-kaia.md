---
description: >-
  Example code for the eth_estimateGas JSON RPC method. Complete guide on how to
  use eth_estimateGas JSON RPC in GetBlock Web3 documentation.
---

# eth\_estimategas

This method returns an estimate of the gas required to execute a transaction, without submitting it. The estimate accounts for the current state and is commonly padded before sending.

## Parameters

| Parameter      | Type   | Required | Description                                             |
| -------------- | ------ | -------- | ------------------------------------------------------- |
| transaction    | object | Yes      | Transaction call object                                 |
| blockParameter | string | No       | Block number in hex, or "latest", "earliest", "pending" |

### Transaction Object

| Field    | Type   | Required | Description                           |
| -------- | ------ | -------- | ------------------------------------- |
| from     | string | No       | 20-byte sender address                |
| to       | string | Yes      | 20-byte recipient or contract address |
| gas      | string | No       | Gas limit for the call (hex)          |
| gasPrice | string | No       | Gas price in wei (hex)                |
| value    | string | No       | Value to send in wei (hex)            |
| data     | string | No       | Encoded function call data            |

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
    "params": [{"from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "to": "0x19aac5f612f524b754ca7e7c41cbfa2e981a4432", "value": "0xde0b6b3a7640000"}],
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
    params: [{"from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "to": "0x19aac5f612f524b754ca7e7c41cbfa2e981a4432", "value": "0xde0b6b3a7640000"}],
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
        'params': [{"from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "to": "0x19aac5f612f524b754ca7e7c41cbfa2e981a4432", "value": "0xde0b6b3a7640000"}],
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
            "params": [{"from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "to": "0x19aac5f612f524b754ca7e7c41cbfa2e981a4432", "value": "0xde0b6b3a7640000"}],
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

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| id        | string | Request identifier matching the request |
| result    | string | Hex-encoded estimated gas units         |

## Use Cases

* **Gas Budgeting**: Estimate gas before building a transaction
* **Fee Preview**: Show a user the expected cost of an action
* **Revert Detection**: Detect a call that would revert before submitting
* **Batch Sizing**: Size a batch of operations under the block gas limit

## Error Handling

| Error Code | Message            | Description                                   |
| ---------- | ------------------ | --------------------------------------------- |
| -32602     | Invalid params     | Invalid transaction object or block parameter |
| -32603     | Internal error     | Contract execution error                      |
| 3          | Execution reverted | The contract reverted the call                |
| -32000     | Execution error    | Insufficient gas or other execution failure   |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const gas = await provider.estimateGas({
    from: '0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045', to: '0x19aac5f612f524b754ca7e7c41cbfa2e981a4432', value: ethers.parseEther('1.0')
});
console.log(gas);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { kaia } from 'viem/chains';

const client = createPublicClient({ chain: kaia, transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/') });

const gas = await client.estimateGas({
    account: '0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045', to: '0x19aac5f612f524b754ca7e7c41cbfa2e981a4432', value: 1000000000000000000n
});
console.log(gas);
```
{% endcode %}
{% endtab %}
{% endtabs %}
