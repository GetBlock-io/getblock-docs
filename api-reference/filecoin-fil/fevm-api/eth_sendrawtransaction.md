# eth\_sendrawtransaction

This method submits a signed, serialized transaction to the Filecoin network. On acceptance it returns the transaction hash; the transaction is then propagated to validators for inclusion.

## Parameters

| Parameter         | Type   | Required | Description                              |
| ----------------- | ------ | -------- | ---------------------------------------- |
| signedTransaction | string | Yes      | The signed transaction data, hex-encoded |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_sendRawTransaction",
    "params": ["0x02f8730182... signed raw transaction hex ...c001a0"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_sendRawTransaction',
    params: ["0x02f8730182... signed raw transaction hex ...c001a0"],
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
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'eth_sendRawTransaction',
        'params': ["0x02f8730182... signed raw transaction hex ...c001a0"],
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
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "eth_sendRawTransaction",
            "params": ["0x02f8730182... signed raw transaction hex ...c001a0"],
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
    "result": "0x4a7b0c3d6e9f2a5b8c1d4e7f0a3b6c9d2e5f8a1b4c7d0e3f6a9b2c5d8e1f4a7b"
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| id        | string | Request identifier matching the request |
| result    | string | Hash of the submitted transaction       |

## Use Cases

* **Payment Broadcast**: Submit a signed native FIL transfer
* **Contract Calls**: Broadcast a signed state-changing contract call
* **dApp Backends**: Relay transactions signed on the client
* **Contract Deployment**: Submit a signed contract-creation transaction

## Error Handling

| Error Code | Message            | Description                                                                     |
| ---------- | ------------------ | ------------------------------------------------------------------------------- |
| -32602     | Invalid params     | The raw transaction is malformed or not valid hex                               |
| -32000     | Execution error    | The transaction was rejected: nonce too low, underpriced, or insufficient funds |
| 3          | Execution reverted | The transaction would revert on execution                                       |
| -32603     | Internal error     | The node failed to broadcast the transaction                                    |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const tx = await provider.broadcastTransaction(signedTx);
console.log(tx.hash);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { filecoin } from 'viem/chains';

const client = createPublicClient({ chain: filecoin, transport: http('https://go.getblock.io/<ACCESS-TOKEN>/') });

const hash = await client.sendRawTransaction({ serializedTransaction: signedTx });
console.log(hash);
```
{% endcode %}
{% endtab %}
{% endtabs %}
