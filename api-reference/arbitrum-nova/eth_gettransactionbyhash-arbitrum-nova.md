# eth\_getTransactionByHash - Arbitrum Nova

This method returns the transaction identified by its 32-byte hash, including its block placement once mined. It is the primary way to look up a specific transaction on GIWA Sepolia.

## Parameters

| Parameter       | Type   | Required | Description              |
| --------------- | ------ | -------- | ------------------------ |
| transactionHash | string | Yes      | 32-byte transaction hash |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByHash",
    "params": ["0x3c8a1f5b2d9e4076c1a8b3d5e7f9021436587a9cbdef012345678abcdef901234"],
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
    method: 'eth_getTransactionByHash',
    params: ['0x3c8a1f5b2d9e4076c1a8b3d5e7f9021436587a9cbdef012345678abcdef901234'],
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
        'method': 'eth_getTransactionByHash',
        'params': ['0x3c8a1f5b2d9e4076c1a8b3d5e7f9021436587a9cbdef012345678abcdef901234'],
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
            "method": "eth_getTransactionByHash",
            "params": ["0x3c8a1f5b2d9e4076c1a8b3d5e7f9021436587a9cbdef012345678abcdef901234"],
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
    "result": {
        "hash": "0x3c8a1f5b2d9e4076c1a8b3d5e7f9021436587a9cbdef012345678abcdef901234",
        "blockHash": "0x9f2b4c1d7e3a6084b5c9d0e1f2a3b4c5d6e7f8091a2b3c4d5e6f708192a3b4c5d",
        "blockNumber": "0x14b8a10",
        "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
        "to": "0x4200000000000000000000000000000000000006",
        "value": "0x0",
        "gas": "0x186a0",
        "type": "0x2",
        "nonce": "0x2a"
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                               |
| --------- | ------ | --------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                         |
| id        | string | Request identifier matching the request                   |
| result    | object | Transaction object, or null if unknown/not yet propagated |

## Use Cases

* **Transaction Lookup**: Fetch full details of a transaction by hash
* **Confirmation Tracking**: Read blockNumber to know when a transaction was mined
* **Wallet History**: Populate a transaction detail view
* **Type Detection**: Read the type field to distinguish legacy, 1559, and deposit transactions

## Error Handling

| Error Code | Message        | Description                             |
| ---------- | -------------- | --------------------------------------- |
| -32602     | Invalid params | Malformed transaction hash              |
| -32603     | Internal error | The node failed to read the transaction |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const tx = await provider.getTransaction('0x3c8a1f5b2d9e4076c1a8b3d5e7f9021436587a9cbdef012345678abcdef901234');
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

const tx = await client.getTransaction({ hash: '0x3c8a1f5b2d9e4076c1a8b3d5e7f9021436587a9cbdef012345678abcdef901234' });
```
{% endcode %}
{% endtab %}
{% endtabs %}
