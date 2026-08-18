---
description: >-
  Example code for the net_version JSON RPC method. Сomplete guide on how to use
  net_version JSON RPC in GetBlock Web3 documentation.
---

# net\_version - Ethereum

This method returns the current network ID as a decimal string. For Ethereum mainnet the value is `"1"`. Network ID historically distinguished chains before the `eth_chainId` method was standardized; modern applications generally prefer `eth_chainId` for chain-selection logic.

## Parameters

This method takes no parameters.

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "net_version",
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

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'net_version',
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'net_version',
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
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "net_version",
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
    "result": "1"
}
```

## Response Parameters

| Parameter | Type   | Description                                                                                                 |
| --------- | ------ | ----------------------------------------------------------------------------------------------------------- |
| `jsonrpc` | string | JSON-RPC protocol version ("2.0")                                                                           |
| `id`      | string | Request identifier matching the request                                                                     |
| `result`  | string | Network ID as a decimal string (`"1"` for Ethereum mainnet, `"11155111"` for Sepolia, `"560048"` for Hoodi) |

## Use Cases

* **Legacy Chain Detection**: Support older tooling that predates `eth_chainId` standardization
* **Cross-Reference Verification**: Sanity-check that `net_version` and `eth_chainId` agree (they generally do on Ethereum-derived chains)
* **Endpoint Environment Discovery**: Detect whether an endpoint points to mainnet vs a testnet without knowing in advance
* **Multi-Chain Dashboards**: Display the network name in an administrative UI by looking up the ID

## Error Handling

| Error Code | Message        | Description                             |
| ---------- | -------------- | --------------------------------------- |
| -32603     | Internal error | Node failed to determine its network ID |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

// Modern approach — prefer getNetwork() which returns rich network info:
const network = await provider.getNetwork();
console.log('Chain ID:', network.chainId.toString());
console.log('Name:', network.name);

// Raw net_version if specifically needed:
const netVersion = await provider.send('net_version', []);
console.log('Network ID:', netVersion);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { mainnet } from 'viem/chains';

const client = createPublicClient({
    chain: mainnet,
    transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/'),
});

// Modern approach — prefer getChainId (uses eth_chainId under the hood):
const chainId = await client.getChainId();
console.log('Chain ID:', chainId);

// Raw net_version if specifically needed:
const netVersion = await client.request({ method: 'net_version' });
console.log('Network ID:', netVersion);
```
{% endcode %}
{% endtab %}
{% endtabs %}
