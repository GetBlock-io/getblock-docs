# eth\_sendRawTransaction - Harmony

This method submits a signed, serialized transaction to the network for inclusion. It is the only write method on a public endpoint: the transaction must be signed client-side before submission.

## Parameters

| Parameter             | Type   | Required | Description                                |
| --------------------- | ------ | -------- | ------------------------------------------ |
| signedTransactionData | string | Yes      | 0x-prefixed RLP-encoded signed transaction |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_sendRawTransaction",
    "params": ["0x02f8b101808459682f008459682f0e830186a094..."],
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
    method: 'eth_sendRawTransaction',
    params: ['0x02f8b101808459682f008459682f0e830186a094...'],
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
        'method': 'eth_sendRawTransaction',
        'params': ['0x02f8b101808459682f008459682f0e830186a094...'],
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
            "method": "eth_sendRawTransaction",
            "params": ["0x02f8b101808459682f008459682f0e830186a094..."],
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
    "result": "0x3c8a1f5b2d9e4076c1a8b3d5e7f9021436587a9cbdef012345678abcdef901234"
}
```

## Response Parameters

| Parameter | Type   | Description                                           |
| --------- | ------ | ----------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                     |
| id        | string | Request identifier matching the request               |
| result    | string | 32-byte transaction hash of the submitted transaction |

## Use Cases

* **Value Transfers**: Broadcast a signed native ONE transfer
* **Contract Calls**: Submit signed state-changing contract interactions
* **Contract Deployment**: Broadcast signed deployment bytecode
* **Batch Submission**: Push a queue of pre-signed transactions in nonce order

## Error Handling

| Error Code | Message              | Description                                                |
| ---------- | -------------------- | ---------------------------------------------------------- |
| -32602     | Invalid params       | Malformed or non-hex signed transaction data               |
| -32000     | Nonce too low        | The transaction nonce is below the account's current nonce |
| -32010     | Transaction rejected | The transaction failed mempool validation                  |
| 3          | Execution reverted   | The transaction reverts on execution                       |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const tx = await wallet.sendTransaction({ to: '0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045', value: ethers.parseEther('0.01') });
await tx.wait();
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';

const client = createPublicClient({
  transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/')
});

const hash = await walletClient.sendRawTransaction({ serializedTransaction });
```
{% endcode %}
{% endtab %}
{% endtabs %}
