---
description: >-
  Example code for the eth_call json-rpc method. Сomplete guide on how to use
  eth_call json-rpc in GetBlock.io Web3 documentation.
---

# eth\_call - Polygon

The **eth\_call** method executes a new message call immediately without creating a transaction on the blockchain. This is the primary method for reading data from smart contracts, including token balances, contract state, and other view/pure functions. The call is executed against the state at the specified block.

## Parameters

| Parameter      | Type   | Required | Description                                                                  |
| -------------- | ------ | -------- | ---------------------------------------------------------------------------- |
| transaction    | object | Yes      | Transaction call object                                                      |
| blockParameter | string | Yes      | Block number in hex, or "latest", "earliest", "pending", "safe", "finalized" |

### Transaction Object

| Field    | Type   | Required | Description                              |
| -------- | ------ | -------- | ---------------------------------------- |
| from     | string | No       | The address the transaction is sent from |
| to       | string | Yes      | The contract address to call             |
| gas      | string | No       | Gas limit for the call (hex)             |
| gasPrice | string | No       | Gas price in wei (hex)                   |
| value    | string | No       | Value to send in wei (hex)               |
| data     | string | No       | Encoded function call data               |

## Request

{% tabs %}
{% tab title="First Tab" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_call",
    "params": [{
        "to": "0x7D1AfA7B718fb893dB30A3aBc0Cfc608AaCfeBB0",
        "data": "0x70a082310000000000000000000000005aAeb6053F3E94C9b9A09f33669435E7Ef1BeAed"
    }, "latest"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="JavaScript (Axios)" overflow="wrap" %}
```javascript
import axios from 'axios';

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'eth_call',
    params: [{
        to: '0x7D1AfA7B718fb893dB30A3aBc0Cfc608AaCfeBB0',
        data: '0x70a082310000000000000000000000005aAeb6053F3E94C9b9A09f33669435E7Ef1BeAed'
    }, 'latest'],
    id: 'getblock.io'
};

axios.post(url, payload, {
    headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log('Result:', response.data.result))
.catch(error => console.error(error));
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="Python (requests)" overflow="wrap" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "eth_call",
    "params": [{
        "to": "0x7D1AfA7B718fb893dB30A3aBc0Cfc608AaCfeBB0",
        "data": "0x70a082310000000000000000000000005aAeb6053F3E94C9b9A09f33669435E7Ef1BeAed"
    }, "latest"],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="Rust (reqwest)" %}
```rust
use reqwest::header;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = reqwest::Client::new();
    
    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header(header::CONTENT_TYPE, "application/json")
        .body(r#"{
            "jsonrpc": "2.0",
            "method": "eth_call",
            "params": [{
                "to": "0x7D1AfA7B718fb893dB30A3aBc0Cfc608AaCfeBB0",
                "data": "0x70a082310000000000000000000000005aAeb6053F3E94C9b9A09f33669435E7Ef1BeAed"
            }, "latest"],
            "id": "getblock.io"
        }"#)
        .send()
        .await?;
    
    println!("{}", response.text().await?);
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
    "result": "0x0000000000000000000000000000000000000000000000056bc75e2d63100000"
}
```

## Response Parameters

| Field   | Type   | Description                                                      |
| ------- | ------ | ---------------------------------------------------------------- |
| jsonrpc | string | JSON-RPC version (2.0)                                           |
| id      | string | Request identifier                                               |
| result  | string | The return value of the executed contract function (hex-encoded) |

## Use Case

The eth\_call method is essential for:

* **Reading token balances**: Query ERC-20/ERC-721 token balances
* **Contract state queries**: Read any public/external view or pure function
* **Simulating transactions**: Test transaction outcomes before sending
* **Price feeds**: Query oracle contracts for current prices
* **DEX quotes**: Get swap quotes from decentralized exchanges

### Example: Reading ERC-20 Token Balance

{% code title="JavaScript (ethers)" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// ERC-20 balanceOf function selector: 0x70a08231
const tokenAddress = '0x7D1AfA7B718fb893dB30A3aBc0Cfc608AaCfeBB0'; // MATIC token
const walletAddress = '0x5aAeb6053F3E94C9b9A09f33669435E7Ef1BeAed';

// Encode the function call
const iface = new ethers.Interface(['function balanceOf(address) view returns (uint256)']);
const data = iface.encodeFunctionData('balanceOf', [walletAddress]);

const result = await provider.call({
    to: tokenAddress,
    data: data
});

const balance = ethers.getBigInt(result);
console.log('Token Balance:', ethers.formatEther(balance));
```
{% endcode %}

## Error Handling

| Status Code | Error Message      | Cause                                         |
| ----------- | ------------------ | --------------------------------------------- |
| 403         | Forbidden          | Missing or invalid ACCESS-TOKEN               |
| -32000      | execution reverted | Contract execution failed                     |
| -32000      | insufficient funds | Simulated transaction requires more value     |
| -32602      | Invalid params     | Invalid transaction object or block parameter |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="JavaScript (ethers)" overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// Direct call
const result = await provider.call({
    to: '0x7D1AfA7B718fb893dB30A3aBc0Cfc608AaCfeBB0',
    data: '0x70a082310000000000000000000000005aAeb6053F3E94C9b9A09f33669435E7Ef1BeAed'
});
console.log('Result:', result);

// Using Contract interface (recommended)
const abi = ['function balanceOf(address) view returns (uint256)'];
const contract = new ethers.Contract('0x7D1AfA7B718fb893dB30A3aBc0Cfc608AaCfeBB0', abi, provider);
const balance = await contract.balanceOf('0x5aAeb6053F3E94C9b9A09f33669435E7Ef1BeAed');
console.log('Balance:', balance.toString());
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="JavaScript (viem)" %}
```javascript
import { createPublicClient, http, parseAbi } from 'viem';
import { polygon } from 'viem/chains';

const client = createPublicClient({
    chain: polygon,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

// Using readContract
const balance = await client.readContract({
    address: '0x7D1AfA7B718fb893dB30A3aBc0Cfc608AaCfeBB0',
    abi: parseAbi(['function balanceOf(address) view returns (uint256)']),
    functionName: 'balanceOf',
    args: ['0x5aAeb6053F3E94C9b9A09f33669435E7Ef1BeAed']
});
console.log('Balance:', balance);
```
{% endcode %}
{% endtab %}
{% endtabs %}
