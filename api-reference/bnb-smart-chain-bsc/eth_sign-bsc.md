---
description: >-
  Example code for the eth_sign JSON RPC method. Сomplete guide on how to use
  eth_sign JSON RPC in GetBlock Web3 documentation.
---

# eth\_sign - BSC

The `eth_sign` method requires the account to be unlocked on the node. It is not available on public RPC endpoints. Use client-side signing libraries (e.g., ethers.js, viem) for signing when using public RPCs.

The `eth_sign` method calculates an Ethereum-specific signature for the given data.

## Parameters

| Parameter | Type   | Required | Description          |
| --------- | ------ | -------- | -------------------- |
| address   | string | Yes      | Address to sign with |
| message   | string | Yes      | Data to sign (hex)   |

## Returns

| Field  | Type   | Description              |
| ------ | ------ | ------------------------ |
| result | string | Signature (65 bytes hex) |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_sign",
    "params": [
        "0x8D97689C9818892B700e27F316cc3E41e17fBeb9",
        "0x48656c6c6f20576f726c64"
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

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'eth_sign',
    params: [
        '0x8D97689C9818892B700e27F316cc3E41e17fBeb9',
        '0x48656c6c6f20576f726c64'
    ],
    id: 'getblock.io'
};

axios.post(url, payload, {
    headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log(response.data))
.catch(error => console.error(error));
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "eth_sign",
    "params": [
        "0x8D97689C9818892B700e27F316cc3E41e17fBeb9",
        "0x48656c6c6f20576f726c64"
    ],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="main.rs" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let payload = json!({
        "jsonrpc": "2.0",
        "method": "eth_sign",
        "params": [
            "0x8D97689C9818892B700e27F316cc3E41e17fBeb9",
            "0x48656c6c6f20576f726c64"
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
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code title="response.json" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "error": {
        "code": -32000,
        "message": "unknown account"
    }
}
```
{% endcode %}

## Response Parameters

| Parameter | Type   | Description              |
| --------- | ------ | ------------------------ |
| result    | string | Signature (if supported) |
| error     | object | Error for public RPCs    |

## Use Cases

* Message signing (local nodes)
* Authentication systems
* Off-chain signatures

## Error Handling

| Error Code | Description          |
| ---------- | -------------------- |
| -32601     | Method not supported |
| -32000     | Account not unlocked |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" %}
```javascript
const { ethers } = require('ethers');

// Sign locally with wallet
const wallet = new ethers.Wallet(privateKey);

const message = 'Hello World';
const signature = await wallet.signMessage(message);
console.log('Signature:', signature);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem.js" %}
```javascript
import { privateKeyToAccount } from 'viem/accounts';

const account = privateKeyToAccount('0x...');

const signature = await account.signMessage({
    message: 'Hello World'
});
console.log('Signature:', signature);
```
{% endcode %}
{% endtab %}
{% endtabs %}
