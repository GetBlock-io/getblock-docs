---
description: >-
  Example code for the sign JSON RPC method. Complete guide to using sign
  JSON-RPC in the GetBlock Web3 documentation.
---

# sign - XRPL

This method takes a transaction in JSON format and a secret value, and returns a signed binary representation of the transaction. This method is admin-only on public servers.

{% hint style="warning" %}
This method is not available on public RPC endpoints. Use local signing with xrpl.js or xrpl-py instead.
{% endhint %}

## Parameters

| Parameter      | Type    | Required | Description         |
| -------------- | ------- | -------- | ------------------- |
| tx\_json       | object  | Yes      | Transaction to sign |
| secret         | string  | No       | Account secret      |
| seed           | string  | No       | Account seed        |
| seed\_hex      | string  | No       | Seed in hex         |
| passphrase     | string  | No       | Passphrase          |
| key\_type      | string  | No       | Key algorithm       |
| offline        | boolean | No       | Offline mode        |
| build\_path    | boolean | No       | Build payment paths |
| fee\_mult\_max | number  | No       | Fee multiplier      |
| fee\_div\_max  | number  | No       | Fee divisor         |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://xrp.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "sign",
    "params": [{
        "tx_json": {
            "TransactionType": "Payment",
            "Account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
            "Destination": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
            "Amount": "1000000"
        },
        "secret": "s..."
    }],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
const axios = require('axios');

const url = 'https://xrp.getblock.io/mainnet/';
const headers = {
    'x-api-key': 'YOUR-API-KEY',
    'Content-Type': 'application/json'
};

// Note: sign method typically not available on public RPCs
// Use local signing instead
const payload = {
    jsonrpc: '2.0',
    method: 'sign',
    params: [{
        tx_json: {
            TransactionType: 'Payment',
            Account: 'rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH',
            Destination: 'ra5nK24KXen9AHvsdFTKHSANinZseWnPcX',
            Amount: '1000000'
        },
        secret: 's...'
    }],
    id: 'getblock.io'
};

axios.post(url, payload, { headers })
    .then(response => console.log(response.data))
    .catch(error => console.error(error));
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

url = "https://xrp.getblock.io/mainnet/"
headers = {
    "x-api-key": "YOUR-API-KEY",
    "Content-Type": "application/json"
}

# Note: sign method typically not available on public RPCs
payload = {
    "jsonrpc": "2.0",
    "method": "sign",
    "params": [{
        "tx_json": {
            "TransactionType": "Payment",
            "Account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
            "Destination": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
            "Amount": "1000000"
        },
        "secret": "s..."
    }],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, json=payload)
print(response.json())
```
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
        "method": "sign",
        "params": [{
            "tx_json": {
                "TransactionType": "Payment",
                "Account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
                "Destination": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
                "Amount": "1000000"
            },
            "secret": "s..."
        }],
        "id": "getblock.io"
    });

    let response = client
        .post("https://xrp.getblock.io/mainnet/")
        .header("x-api-key", "YOUR-API-KEY")
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
    "result": "null",
    "id": "getblock.io",
    "status_code": 405,
    "message": "Method not allowed"
}
```

## Response Parameters

| Field    | Type   | Description            |
| -------- | ------ | ---------------------- |
| tx\_blob | string | Signed transaction hex |
| tx\_json | object | Transaction JSON       |
| hash     | string | Transaction hash       |

## Use Cases

* Transaction signing (admin only)
* Development environments
* Private nodes

## Recommended: Local Signing

{% tabs %}
{% tab title="xrpl.js" %}
```javascript
const xrpl = require('xrpl');

async function signAndSubmit() {
    const client = new xrpl.Client('wss://xrplcluster.com');
    await client.connect();
    
    const wallet = xrpl.Wallet.fromSeed('s...');
    
    const payment = {
        TransactionType: 'Payment',
        Account: wallet.address,
        Destination: 'ra5nK24KXen9AHvsdFTKHSANinZseWnPcX',
        Amount: '1000000'
    };
    
    // Local signing
    const prepared = await client.autofill(payment);
    const signed = wallet.sign(prepared);
    
    console.log('Signed blob:', signed.tx_blob);
    
    // Submit
    const result = await client.submitAndWait(signed.tx_blob);
    console.log('Result:', result);
    
    await client.disconnect();
}

signAndSubmit();
```
{% endtab %}

{% tab title="xrpl-py" %}
```python
from xrpl.clients import JsonRpcClient
from xrpl.wallet import Wallet
from xrpl.models.transactions import Payment
from xrpl.transaction import sign, submit_and_wait

client = JsonRpcClient("https://xrp.getblock.io/mainnet/")
wallet = Wallet.from_seed("s...")

payment = Payment(
    account=wallet.address,
    destination="ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
    amount="1000000"
)

# Local signing
signed = sign(payment, wallet)
print("Signed:", signed)
```
{% endtab %}
{% endtabs %}
