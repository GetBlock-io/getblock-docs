---
description: >-
  Example code for the eth_sendRawTransaction JSON-RPC method. Complete guide on
  how to use eth_sendRawTransaction JSON-RPC in GetBlock Web3 documentation.
---

# eth\_sendRawTransaction - Somnia

This method submits a pre-signed transaction to the Somnia network for execution. This is the primary method for broadcasting transactions. With Somnia's sub-second finality and sub-cent fees, transactions are confirmed rapidly and economically.

## Parameters

| Parameter             | Type   | Required | Description                                 |
| --------------------- | ------ | -------- | ------------------------------------------- |
| signedTransactionData | string | Yes      | The signed transaction data as a hex string |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "eth_sendRawTransaction",
  "params": ["0xf86c098504a817c800825208943535353535353535353535353535353535353535880de0b6b3a76400008025a028ef61340bd939bc2195fe537567866003e1a15d3c71ff63e1590620aa636276a067cbe9d8997f761aecb703304b3800ccf555c9f3dc64214b297fb1966a3b6d83"]
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'eth_sendRawTransaction',
  params: ['0xf86c098504a817c800825208943535353535353535353535353535353535353535880de0b6b3a76400008025a028ef61340bd939bc2195fe537567866003e1a15d3c71ff63e1590620aa636276a067cbe9d8997f761aecb703304b3800ccf555c9f3dc64214b297fb1966a3b6d83']
};

axios.post(url, payload, {
  headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log(response.data))
.catch(error => console.error(error));
```
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "eth_sendRawTransaction",
    "params": ["0xf86c098504a817c800825208943535353535353535353535353535353535353535880de0b6b3a76400008025a028ef61340bd939bc2195fe537567866003e1a15d3c71ff63e1590620aa636276a067cbe9d8997f761aecb703304b3800ccf555c9f3dc64214b297fb1966a3b6d83"]
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let payload = json!({
        "jsonrpc": "2.0",
        "id": "getblock.io",
        "method": "eth_sendRawTransaction",
        "params": ["0xf86c098504a817c800825208943535353535353535353535353535353535353535880de0b6b3a76400008025a028ef61340bd939bc2195fe537567866003e1a15d3c71ff63e1590620aa636276a067cbe9d8997f761aecb703304b3800ccf555c9f3dc64214b297fb1966a3b6d83"]
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

```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"
}
```

## Response Parameters

| Parameter | Type   | Description                               |
| --------- | ------ | ----------------------------------------- |
| result    | string | Transaction hash of submitted transaction |

## Use Cases

* Submit signed transactions to the network
* Deploy smart contracts
* Transfer SOMI tokens
* Execute contract functions
* Batch transaction submission

## Error Handling

| Error Code | Description                                             |
| ---------- | ------------------------------------------------------- |
| -32602     | Invalid params - malformed transaction data             |
| -32603     | Internal error - node processing issues                 |
| -32000     | Transaction rejected - insufficient funds, nonce issues |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');
const wallet = new ethers.Wallet('YOUR_PRIVATE_KEY', provider);

const tx = await wallet.sendTransaction({
  to: '0x8626f6940E2eb28930eFb4CeF49B2d1F2C9C1199',
  value: ethers.parseEther('1.0')
});
console.log(tx.hash);
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createWalletClient, http, parseEther } from 'viem';
import { privateKeyToAccount } from 'viem/accounts';
import { somnia } from 'viem/chains';

const account = privateKeyToAccount('0xYOUR_PRIVATE_KEY');

const client = createWalletClient({
  account,
  chain: somnia,
  transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const hash = await client.sendTransaction({
  to: '0x8626f6940E2eb28930eFb4CeF49B2d1F2C9C1199',
  value: parseEther('1')
});
console.log(hash);
```
{% endtab %}
{% endtabs %}
