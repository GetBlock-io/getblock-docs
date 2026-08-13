# eth\_gettransactioncount

This method returns the number of transactions sent from an address, which is the account nonce. It is used to set the nonce for the next transaction.

## Parameters

| Parameter      | Type   | Required | Description                                             |
| -------------- | ------ | -------- | ------------------------------------------------------- |
| address        | string | Yes      | 20-byte address to read the nonce of                    |
| blockParameter | string | Yes      | Block number in hex, or "latest", "earliest", "pending" |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
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

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_getTransactionCount',
    params: ["0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "latest"],
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
        'method': 'eth_getTransactionCount',
        'params': ["0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "latest"],
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

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| id        | string | Request identifier matching the request |
| result    | string | Hex-encoded transaction count (nonce)   |

## Use Cases

* **Nonce Management**: Set the nonce for the next outgoing transaction
* **Transaction Ordering**: Sequence a batch of transactions from one account
* **Stuck Transactions**: Detect a gap between latest and pending nonces
* **Account Activity**: Gauge how many transactions an account has sent

## Error Handling

| Error Code | Message        | Description                                                                |
| ---------- | -------------- | -------------------------------------------------------------------------- |
| -32602     | Invalid params | The address is not a valid 20-byte hex string, or the block tag is invalid |
| -32603     | Internal error | The node failed to read the requested state                                |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const nonce = await provider.getTransactionCount('0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045');
console.log(nonce);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { kaia } from 'viem/chains';

const client = createPublicClient({ chain: kaia, transport: http('https://go.getblock.io/<ACCESS-TOKEN>/') });

const nonce = await client.getTransactionCount({ address: '0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045' });
console.log(nonce);
```
{% endcode %}
{% endtab %}
{% endtabs %}
