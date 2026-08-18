---
description: >-
  Example code for the eth_chainId JSON RPC method. Сomplete guide on how to use
  eth_chainId JSON RPC in GetBlock Web3 documentation.
---

# eth\_chainId - Ethereum

This method returns the currently configured chain ID as a hex-encoded integer. Chain ID is defined by EIP-155 and is the value applications sign into transactions to prevent replay across chains. Ethereum mainnet returns `0x1`, Sepolia returns `0xaa36a7`, and Hoodi returns `0x88bb0`.

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

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
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
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/")
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
    "result": "0x1"
}
```

## Response Parameters

| Parameter | Type   | Description                                                                              |
| --------- | ------ | ---------------------------------------------------------------------------------------- |
| `jsonrpc` | string | JSON-RPC protocol version ("2.0")                                                        |
| `id`      | string | Request identifier matching the request                                                  |
| `result`  | string | Hex-encoded chain ID (`0x1` = Ethereum mainnet, `0xaa36a7` = Sepolia, `0x88bb0` = Hoodi) |

## Use Cases

* **Transaction Signing**: Wallets and signers include the chain ID in signed transactions per EIP-155 to prevent replay attacks across chains
* **Chain-Selection Logic**: dApps confirm the connected endpoint matches the intended network before initiating a write
* **Multi-Chain Router Behavior**: Cross-chain aggregators verify the target chain before sending calldata to the correct contract
* **Wallet UX**: Prompt users to switch networks when the connected chain ID doesn't match the dApp's expected chain

## Error Handling

| Error Code | Message        | Description                                   |
| ---------- | -------------- | --------------------------------------------- |
| -32603     | Internal error | Node failed to return its configured chain ID |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const network = await provider.getNetwork();
console.log('Chain ID:', network.chainId.toString());  // "1" for Ethereum mainnet
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

const chainId = await client.getChainId();
console.log('Chain ID:', chainId);  // 1 for Ethereum mainnet
```
{% endcode %}
{% endtab %}
{% endtabs %}
