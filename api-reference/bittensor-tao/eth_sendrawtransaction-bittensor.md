# eth\_sendrawtransaction bittensor

{% hint style="info" %}
**Subtensor EVM JSON-RPC method.** Call against a GetBlock endpoint configured for the **EVM interface** (not the Substrate interface). Subtensor EVM uses chain ID **945** (`0x3b1`). Standard Ethereum tooling — ethers.js, viem, Hardhat, Foundry, MetaMask — works unmodified.
{% endhint %}

Submits a signed EVM transaction to Subtensor EVM. Transaction must be RLP-encoded and signed with chain ID `945` (`0x3b1`) for EIP-155 replay protection.

## Parameters

| Parameter | Type | Required | Description                              |
| --------- | ---- | -------- | ---------------------------------------- |
| signedTx  | DATA | Yes      | RLP-encoded signed EVM transaction (hex) |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_sendRawTransaction",
    "params": [
        "0xf86c80843b9aca0082520894d8da6bf26964af9d7eed9e03e53415d37aa9604588016345785d8a000080820722a01b5e176d927f8e9ab405058b2d2457392da3e20f328b16ddabcebc33eaac5fea3094a6d52d77b6dc9c4f74e58fb50a76b80e3a8b1f47"
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
        "0xf86c80843b9aca0082520894d8da6bf26964af9d7eed9e03e53415d37aa9604588016345785d8a000080820722a01b5e176d927f8e9ab405058b2d2457392da3e20f328b16ddabcebc33eaac5fea3094a6d52d77b6dc9c4f74e58fb50a76b80e3a8b1f47"
    ],
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/',
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

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_sendRawTransaction",
    "params": [
        "0xf86c80843b9aca0082520894d8da6bf26964af9d7eed9e03e53415d37aa9604588016345785d8a000080820722a01b5e176d927f8e9ab405058b2d2457392da3e20f328b16ddabcebc33eaac5fea3094a6d52d77b6dc9c4f74e58fb50a76b80e3a8b1f47"
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
                "0xf86c80843b9aca0082520894d8da6bf26964af9d7eed9e03e53415d37aa9604588016345785d8a000080820722a01b5e176d927f8e9ab405058b2d2457392da3e20f328b16ddabcebc33eaac5fea3094a6d52d77b6dc9c4f74e58fb50a76b80e3a8b1f47"
        ],
        "id": "getblock.io"
});

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
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
    "result": "0xa8f9b3c7d2e4f6a1b9c8d7e6f5a4b3c2d1e0f9a8b7c6d5e4f3a2b1c0d9e8f7a6"
}
```
{% endcode %}

## Response Parameters

| Field  | Type | Description                                                           |
| ------ | ---- | --------------------------------------------------------------------- |
| result | DATA | Transaction hash — use `eth_getTransactionReceipt` to track inclusion |

## Use Cases

* MetaMask transaction submission
* Smart contract deployment on Bittensor EVM
* Server-side relayer transaction submission

## Error Handling

| Status Code | Error Message      | Cause                                                                                                                      |
| ----------- | ------------------ | -------------------------------------------------------------------------------------------------------------------------- |
| 403         | Forbidden          | Missing or invalid `<ACCESS-TOKEN>`                                                                                        |
| -32602      | Invalid params     | Request parameters are missing or malformed                                                                                |
| -32601      | Method not found   | Method does not exist on this interface — check whether you're calling a Substrate method on an EVM endpoint or vice versa |
| -32603      | Internal error     | Server-side error while processing the request                                                                             |
| 429         | Too Many Requests  | Rate limit exceeded for your plan                                                                                          |
| -32000      | Insufficient funds | Sender doesn't have enough TAO to cover gas + value                                                                        |
| -32000      | Nonce too low      | Sender's nonce has advanced — transaction is stale                                                                         |
| -32000      | Invalid chain ID   | Transaction signed with wrong chain ID (must be 945)                                                                       |

## SDK Integration

{% tabs %}
{% tab title="ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// Generic JSON-RPC call (works for any EVM method):
const result = await provider.send('eth_sendRawTransaction', ["0xf86c80843b9aca0082520894d8da6bf26964af9d7eed9e03e53415d37aa9604588016345785d8a000080820722a01b5e176d927f8e9ab405058b2d2457392da3e20f328b16ddabcebc33eaac5fea3094a6d52d77b6dc9c4f74e58fb50a76b80e3a8b1f47"]);
console.log(result);

// Most standard methods have typed wrappers:
// provider.getBalance(addr), provider.getBlock(n), provider.getCode(addr), etc.
```
{% endtab %}

{% tab title="viem" %}
```javascript
import { createPublicClient, http } from 'viem';

const client = createPublicClient({
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const result = await client.request({
    method: 'eth_sendRawTransaction',
    params: ["0xf86c80843b9aca0082520894d8da6bf26964af9d7eed9e03e53415d37aa9604588016345785d8a000080820722a01b5e176d927f8e9ab405058b2d2457392da3e20f328b16ddabcebc33eaac5fea3094a6d52d77b6dc9c4f74e58fb50a76b80e3a8b1f47"]
});
console.log(result);
```
{% endtab %}
{% endtabs %}
