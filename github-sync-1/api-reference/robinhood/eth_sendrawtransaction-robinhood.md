---
description: >-
  Example code for the eth_sendRawTransaction JSON-RPC method. Complete guide to
  using the eth_sendRawTransaction JSON-RPC method in GetBlock.io Web3
  documentation.
---

# eth\_sendRawTransaction - Robinhood

Submits a signed transaction to the network. The transaction must be RLP-encoded and signed with the correct chain ID (`4663` / `0x1237` for Robinhood Chain mainnet) for EIP-155 replay protection.

## Parameters

| Parameter | Type | Required | Description                          |
| --------- | ---- | -------- | ------------------------------------ |
| signedTx  | DATA | Yes      | RLP-encoded signed transaction (hex) |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_sendRawTransaction",
    "params": [
        "0xf86c8085055ae82600825208947a25d0f0ff6a25f36de0a19c3d5f6dc2c6eaed6a88016345785d8a000080820246e2a01b5e176d927f8e9ab405058b2d2457392da3e20f328b16ddabcebc33eaac5fea3094a6d52d77b6dc9c4f74e58fb50a76b80e3a8b1f47"
    ],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code overflow="wrap" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_sendRawTransaction",
    "params": [
        "0xf86c8085055ae82600825208947a25d0f0ff6a25f36de0a19c3d5f6dc2c6eaed6a88016345785d8a000080820246e2a01b5e176d927f8e9ab405058b2d2457392da3e20f328b16ddabcebc33eaac5fea3094a6d52d77b6dc9c4f74e58fb50a76b80e3a8b1f47"
    ],
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/',
    headers: {
        'Content-Type': 'application/json'
    },
    data: data
};

axios(config)
    .then(response => console.log(JSON.stringify(response.data, null, 2)))
    .catch(error => console.log(error));
```
{% endcode %}
{% endtab %}

{% tab title="Python (Requests)" %}
```python
import requests
import json

url = "https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_sendRawTransaction",
    "params": [
        "0xf86c8085055ae82600825208947a25d0f0ff6a25f36de0a19c3d5f6dc2c6eaed6a88016345785d8a000080820246e2a01b5e176d927f8e9ab405058b2d2457392da3e20f328b16ddabcebc33eaac5fea3094a6d52d77b6dc9c4f74e58fb50a76b80e3a8b1f47"
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
                "0xf86c8085055ae82600825208947a25d0f0ff6a25f36de0a19c3d5f6dc2c6eaed6a88016345785d8a000080820246e2a01b5e176d927f8e9ab405058b2d2457392da3e20f328b16ddabcebc33eaac5fea3094a6d52d77b6dc9c4f74e58fb50a76b80e3a8b1f47"
        ],
        "id": "getblock.io"
});

    let response = client
        .post("https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/")
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

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0xa8f9b3c7d2e4f6a1b9c8d7e6f5a4b3c2d1e0f9a8b7c6d5e4f3a2b1c0d9e8f7a6"
}
```

## Response Parameters

| Field  | Type | Description                                                           |
| ------ | ---- | --------------------------------------------------------------------- |
| result | DATA | Transaction hash — use `eth_getTransactionReceipt` to track inclusion |

## Use Cases

* Wallet transaction submission
* DEX backends submitting orders on Robinhood Chain DEXes (Uniswap, Lighter, 1inch)
* Stock Token transfer submission

## Error Handling

<table data-search="false"><thead><tr><th>Status Code</th><th>Error Message</th><th>Cause</th></tr></thead><tbody><tr><td>403</td><td>Forbidden</td><td>Missing or invalid <code>&#x3C;ACCESS-TOKEN></code></td></tr><tr><td>-32602</td><td>Invalid params</td><td>Request parameters are missing or malformed</td></tr><tr><td>-32603</td><td>Internal error</td><td>Server-side error while processing the request</td></tr><tr><td>429</td><td>Too Many Requests</td><td>Rate limit exceeded for your plan</td></tr><tr><td>-32000</td><td>Transaction underpriced</td><td>Gas price below the minimum the pool accepts</td></tr><tr><td>-32000</td><td>Already known</td><td>Transaction was already submitted</td></tr><tr><td>-32000</td><td>Nonce too low</td><td>Sender's nonce has advanced — this transaction is stale</td></tr><tr><td>-32000</td><td>Insufficient funds</td><td>Sender doesn't have enough ETH to cover gas × gasLimit + value</td></tr><tr><td>-32000</td><td>Intrinsic gas too low</td><td>Gas limit below the minimum for this transaction type</td></tr></tbody></table>

## SDK Integration

{% tabs %}
{% tab title="ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/');

// Generic JSON-RPC call (works for any method):
const result = await provider.send('eth_sendRawTransaction', ["0xf86c8085055ae82600825208947a25d0f0ff6a25f36de0a19c3d5f6dc2c6eaed6a88016345785d8a000080820246e2a01b5e176d927f8e9ab405058b2d2457392da3e20f328b16ddabcebc33eaac5fea3094a6d52d77b6dc9c4f74e58fb50a76b80e3a8b1f47"]);
console.log(result);

// Many standard methods have typed wrappers on ethers Provider,
// e.g. provider.getBalance(addr), provider.getBlock(n), provider.getTransaction(hash).
```
{% endtab %}

{% tab title="viem" %}
```javascript
import { createPublicClient, http } from 'viem';

const client = createPublicClient({
    transport: http('https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/')
});

// Generic JSON-RPC request (works for any method):
const result = await client.request({
    method: 'eth_sendRawTransaction',
    params: ["0xf86c8085055ae82600825208947a25d0f0ff6a25f36de0a19c3d5f6dc2c6eaed6a88016345785d8a000080820246e2a01b5e176d927f8e9ab405058b2d2457392da3e20f328b16ddabcebc33eaac5fea3094a6d52d77b6dc9c4f74e58fb50a76b80e3a8b1f47"]
});
console.log(result);

// Many standard methods have typed wrappers on PublicClient,
// e.g. client.getBalance({ address }), client.getBlock({ blockNumber }).
```
{% endtab %}
{% endtabs %}
