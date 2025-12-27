---
description: >-
  Example code for the eth_sendRawTransaction JSON-RPC method. Complete guide on
  how to use eth_sendRawTransaction JSON-RPC in GetBlock Web3 documentation.
---

# eth\_sendRawTransaction - Monad

This method submits a pre-signed transaction for broadcast to the Monad network.

## Parameters

| Parameter | Type   | Required | Description                                  |
| --------- | ------ | -------- | -------------------------------------------- |
| data      | string | Yes      | The signed transaction data as a hex string. |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_sendRawTransaction",
    "params": ["0xf86c0a8502540be400825208944bbeeb066ed09b7aed07bf39eee0460dfa26152088016345785d8a0000801ba0e2c.."],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="sendRawTx.js" %}
```javascript
import axios from 'axios';

const signedTx = "0xf86c0a8502540be400825208944bbeeb066ed09b7aed07bf39eee0460dfa26152088016345785d8a0000801ba0e2c..";

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_sendRawTransaction",
    "params": [signedTx],
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
    .then(response => console.log(JSON.stringify(response.data)))
    .catch(error => console.log(error));
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="send_raw_tx.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

signed_tx = "0xf86c0a8502540be400825208944bbeeb066ed09b7aed07bf39eee0460dfa26152088016345785d8a0000801ba0e2c.."

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_sendRawTransaction",
    "params": [signed_tx],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="main.rs" %}
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
            "method": "eth_sendRawTransaction",
            "params": ["0xf86c0a8502540be400825208944bbeeb066ed09b7aed07bf39eee0460dfa26152088016345785d8a0000801ba0e2c.."],
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

{% code title="response.json" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331"
}
```
{% endcode %}

## Response Parameters

| Field  | Type   | Description                                                                     |
| ------ | ------ | ------------------------------------------------------------------------------- |
| result | string | The 32-byte transaction hash, or zero hash if transaction is not yet available. |

## Use Case

The `eth_sendRawTransaction` method is essential for:

* Submitting signed transactions to the network
* Wallet transaction broadcasting
* DeFi protocol interactions
* Token transfers
* Smart contract deployments
* High-frequency trading on Monad

## Error Handling

| Status Code | Error Message        | Cause                                |
| ----------- | -------------------- | ------------------------------------ |
| 403         | Forbidden            | Missing or invalid ACCESS-TOKEN.     |
| -32602      | Invalid params       | Invalid transaction data format.     |
| -32000      | Nonce too low        | Transaction nonce already used.      |
| -32000      | Insufficient funds   | Not enough MON for gas.              |
| -32000      | Gas limit exceeded   | Transaction exceeds block gas limit. |
| -32003      | Transaction rejected | Transaction validation failed.       |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-send.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');
const wallet = new ethers.Wallet(privateKey, provider);

const tx = {
    to: '0x742d35Cc6634C0532925a3b844Bc9e7595f0bEb',
    value: ethers.parseEther('1.0'),
    gasLimit: 21000
};

const txResponse = await wallet.sendTransaction(tx);
console.log('Transaction hash:', txResponse.hash);

// Wait for confirmation (fast on Monad!)
const receipt = await txResponse.wait();
console.log('Confirmed in block:', receipt.blockNumber);
```
{% endcode %}
{% endtab %}
{% endtabs %}
