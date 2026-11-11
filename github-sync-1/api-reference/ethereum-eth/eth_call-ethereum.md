---
description: >-
  Example code for the eth_call JSON RPC method. Сomplete guide on how to use
  eth_call JSON RPC in GetBlock Web3 documentation.
---

# eth\_call - Ethereum

This method executes a message call immediately against the current state without creating a transaction on the blockchain. It's the primary method for reading data from smart contracts — including token balances, contract state, and view/pure function results — and for simulating transactions before broadcasting.

## Parameters

| Parameter        | Type   | Required | Description                                                                      |
| ---------------- | ------ | -------- | -------------------------------------------------------------------------------- |
| `transaction`    | object | Yes      | Transaction call object (see below)                                              |
| `blockParameter` | string | Yes      | Block number in hex, or `latest`, `earliest`, `pending`, `finalized`, `safe`     |
| `stateOverride`  | object | No       | Optional state overrides applied before executing the call (advanced simulation) |

### Transaction Object

| Field                  | Type   | Required | Description                         |
| ---------------------- | ------ | -------- | ----------------------------------- |
| `from`                 | string | No       | 20-byte sender address              |
| `to`                   | string | Yes      | 20-byte recipient/contract address  |
| `gas`                  | string | No       | Gas limit for the call (hex)        |
| `gasPrice`             | string | No       | Gas price in wei (hex, legacy)      |
| `maxFeePerGas`         | string | No       | EIP-1559 max fee per gas (hex)      |
| `maxPriorityFeePerGas` | string | No       | EIP-1559 priority fee per gas (hex) |
| `value`                | string | No       | Value to send in wei (hex)          |
| `data`                 | string | No       | Encoded function call data          |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_call",
    "params": [
        {
            "to": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
            "data": "0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045"
        },
        "latest"
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
    method: 'eth_call',
    params: [
        {
            "to": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
            "data": "0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045"
        },
        "latest"
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
        'method': 'eth_call',
        'params': [
        {
            "to": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
            "data": "0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045"
        },
        "latest"
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
            "method": "eth_call",
            "params": [
        {
            "to": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
            "data": "0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045"
        },
        "latest"
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
    "result": "0x0000000000000000000000000000000000000000000000000de0b6b3a7640000"
}
```

## Response Parameters

| Parameter | Type   | Description                                    |
| --------- | ------ | ---------------------------------------------- |
| `jsonrpc` | string | JSON-RPC protocol version ("2.0")              |
| `id`      | string | Request identifier matching the request        |
| `result`  | string | Hex-encoded return data from the contract call |

## Use Cases

* **Token Balances**: Query ERC-20 token balances via `balanceOf(address)`
* **Contract State**: Read public state variables and view/pure function results from smart contracts
* **Price Feeds**: Query Chainlink oracle prices, Uniswap pool prices, or other DeFi rate sources
* **NFT Metadata**: Read `ownerOf`, `tokenURI`, and other ERC-721/ERC-1155 metadata
* **DeFi Position Snapshots**: Query pool reserves, exchange rates, liquidation health factors
* **Simulation Before Send**: Test a transaction against current state to detect reverts before broadcasting

## Error Handling

| Error Code | Message            | Description                                                                                                   |
| ---------- | ------------------ | ------------------------------------------------------------------------------------------------------------- |
| -32602     | Invalid params     | Transaction object or block parameter is malformed                                                            |
| 3          | Execution reverted | The contract call reverted — decode the return data as an Error(string) or custom error to inspect the reason |
| -32000     | Execution error    | Insufficient gas, out-of-gas, or other EVM execution failure                                                  |
| -32603     | Internal error     | Node failed to execute the call                                                                               |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

// High-level: use a Contract instance for typed reads
const WETH_ABI = ['function balanceOf(address) view returns (uint256)'];
const weth = new ethers.Contract('0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2', WETH_ABI, provider);
const balance = await weth.balanceOf('0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045');
console.log('WETH balance:', ethers.formatEther(balance));
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" overflow="wrap" %}
```javascript
import { createPublicClient, http, formatEther } from 'viem';
import { mainnet } from 'viem/chains';

const client = createPublicClient({
    chain: mainnet,
    transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/'),
});

// High-level: readContract with typed ABI
const balance = await client.readContract({
    address: '0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2',
    abi: [{ name: 'balanceOf', type: 'function', stateMutability: 'view', inputs: [{ type: 'address' }], outputs: [{ type: 'uint256' }] }],
    functionName: 'balanceOf',
    args: ['0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045'],
});
console.log('WETH balance:', formatEther(balance));
```
{% endcode %}
{% endtab %}
{% endtabs %}
