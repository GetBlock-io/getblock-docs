# eth\_chainid

This method returns the chain ID of the Kaia network, used to sign replay-protected transactions under EIP-155. Kaia Mainnet returns 0x2019 (8217).

## Parameters

This method does not accept any parameters.

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_chainId",
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

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_chainId',
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
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'eth_chainId',
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
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "eth_chainId",
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
    "result": "0x2019"
}
```

## Response Parameters

| Parameter | Type   | Description                                           |
| --------- | ------ | ----------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                     |
| id        | string | Request identifier matching the request               |
| result    | string | Hex-encoded chain ID (0x2019 = 8217 for Kaia Mainnet) |

## Use Cases

* **Replay Protection**: Include the chain ID when signing EIP-155 transactions
* **Network Detection**: Confirm a wallet is connected to Kaia mainnet
* **Multi-Chain Routing**: Select network-specific logic by chain ID
* **Configuration Checks**: Validate an RPC endpoint serves the expected chain

## Error Handling

| Error Code | Message        | Description                                            |
| ---------- | -------------- | ------------------------------------------------------ |
| -32602     | Invalid params | A parameter is missing or has the wrong type or format |
| -32603     | Internal error | The node failed to process the request                 |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const network = await provider.getNetwork();
console.log(network.chainId);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { kaia } from 'viem/chains';

const client = createPublicClient({ chain: kaia, transport: http('https://go.getblock.io/<ACCESS-TOKEN>/') });

const chainId = await client.getChainId();
console.log(chainId);
```
{% endcode %}
{% endtab %}
{% endtabs %}
