---
description: >-
  Example code for the eth_sendRawTransaction JSON-RPC method. Complete guide on
  how to use eth_sendRawTransaction JSON-RPC in GetBlock Web3 documentation.
---

# eth\_sendRawTransaction - AVAX

Broadcasts a signed transaction to the C-Chain. The primary method for submitting any state-changing operation — AVAX transfers, ERC-20 transfers, smart contract calls, and contract deployments.

## Parameters

| Parameter    | Type   | Required | Description                                                                                         |
| ------------ | ------ | -------- | --------------------------------------------------------------------------------------------------- |
| signedTxData | string | Yes      | The signed transaction data (hex), produced offline by a signer such as ethers.js, viem, or web3.py |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_sendRawTransaction",
    "params": [
        "0xf86c808504a817c80082520894d85498dbeaeb1df24be52eed4f52eac2fbd56245880de0b6b3a76400008085a04f3a1c..."
    ],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_sendRawTransaction",
    "params": [
        "0xf86c808504a817c80082520894d85498dbeaeb1df24be52eed4f52eac2fbd56245880de0b6b3a76400008085a04f3a1c..."
    ],
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc',
    headers: {
        'Content-Type': 'application/json'
    },
    data: data
};

axios(config)
    .then(response => console.log(JSON.stringify(response.data, null, 2)))
    .catch(error => console.log(error));
```
{% endtab %}

{% tab title="Python (Requests)" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_sendRawTransaction",
    "params": [
        "0xf86c808504a817c80082520894d85498dbeaeb1df24be52eed4f52eac2fbd56245880de0b6b3a76400008085a04f3a1c..."
    ],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endtab %}

{% tab title="Rust (Reqwest)" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let payload = json!({
        "jsonrpc": "2.0",
        "method": "eth_sendRawTransaction",
        "params": [
                "0xf86c808504a817c80082520894d85498dbeaeb1df24be52eed4f52eac2fbd56245880de0b6b3a76400008085a04f3a1c..."
        ],
        "id": "getblock.io"
});

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc")
        .header("Content-Type", "application/json")
        .json(&payload)
        .send()
        .await?;

    let result: serde_json::Value = response.json().await?;
    println!("{:#?}", result);

    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"
}
```
{% endcode %}

## Response Parameters

| Field  | Type   | Description                                                               |
| ------ | ------ | ------------------------------------------------------------------------- |
| result | string | Transaction hash, used to track inclusion via `eth_getTransactionReceipt` |

## Use Cases

* Send AVAX transfers on C-Chain
* Execute ERC-20 token transfers
* Interact with DeFi protocols (Trader Joe, Pangolin, etc.)
* Deploy smart contracts

## Error Handling

| Status Code | Error Message                       | Cause                                                 |
| ----------- | ----------------------------------- | ----------------------------------------------------- |
| 404         | Not Found                           | Missing or invalid `<ACCESS-TOKEN>`                   |
| -32602      | Invalid params                      | Request parameters are missing or malformed           |
| 429         | Too Many Requests                   | Rate limit exceeded for your plan                     |
| -32000      | Insufficient funds for gas          | Sender does not have enough AVAX to pay gas + value   |
| -32000      | Nonce too low                       | Transaction's nonce has already been used             |
| -32000      | Replacement transaction underpriced | Replacement transaction must offer a higher gas price |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc');
const wallet = new ethers.Wallet(privateKey, provider);

const tx = await wallet.sendTransaction({
    to: '0xd85498dbeaeb1df24be52eed4f52eac2fbd56245',
    value: ethers.parseEther('0.1')
});
console.log('Tx Hash:', tx.hash);
await tx.wait();
console.log('Confirmed!');
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createWalletClient, http, parseEther } from 'viem';
import { avalanche } from 'viem/chains';
import { privateKeyToAccount } from 'viem/accounts';

const account = privateKeyToAccount('0x...');

const client = createWalletClient({
    account,
    chain: avalanche,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc')
});

const hash = await client.sendTransaction({
    to: '0xd85498dbeaeb1df24be52eed4f52eac2fbd56245',
    value: parseEther('0.1')
});
console.log('Tx Hash:', hash);
```
{% endtab %}
{% endtabs %}
