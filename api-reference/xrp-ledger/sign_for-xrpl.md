---
description: >-
  Example code for the sign_for JSON RPC method. Complete guide to using sign_fo
  JSON-RPC in the GetBlock Web3 documentation.
---

# sign\_for - XRPL

This method provides one signature for a multi-signed transaction. This method is admin-only on public servers.

{% hint style="warning" %}
Note: This method is not available on public RPC endpoints. Use local multi-signing with xrpl.js or xrpl-py instead.
{% endhint %}

### Parameters

| Parameter  | Type   | Required | Description         |
| ---------- | ------ | -------- | ------------------- |
| account    | string | Yes      | Address to sign for |
| tx\_json   | object | Yes      | Transaction to sign |
| secret     | string | No       | Signer's secret     |
| seed       | string | No       | Signer's seed       |
| seed\_hex  | string | No       | Seed in hex         |
| passphrase | string | No       | Passphrase          |
| key\_type  | string | No       | Key algorithm       |

### Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="curl" %}
```bash
curl --location --request POST 'https://xrp.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "sign_for",
    "params": [{
        "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
        "tx_json": {
            "TransactionType": "Payment",
            "Account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
            "Destination": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
            "Amount": "1000000",
            "Fee": "30"
        },
        "secret": "s..."
    }],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="sign_for.js" %}
```javascript
const axios = require('axios');

const url = 'https://xrp.getblock.io/mainnet/';
const headers = {
    'x-api-key': 'YOUR-API-KEY',
    'Content-Type': 'application/json'
};

const payload = {
    jsonrpc: '2.0',
    method: 'sign_for',
    params: [{
        account: 'rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH',
        tx_json: {
            TransactionType: 'Payment',
            Account: 'rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH',
            Destination: 'ra5nK24KXen9AHvsdFTKHSANinZseWnPcX',
            Amount: '1000000',
            Fee: '30'
        },
        secret: 's...'
    }],
    id: 'getblock.io'
};

axios.post(url, payload, { headers })
    .then(response => console.log(response.data))
    .catch(error => console.error(error));
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="sign_for.py" %}
```python
import requests

url = "https://xrp.getblock.io/mainnet/"
headers = {
    "x-api-key": "YOUR-API-KEY",
    "Content-Type": "application/json"
}

payload = {
    "jsonrpc": "2.0",
    "method": "sign_for",
    "params": [{
        "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
        "tx_json": {
            "TransactionType": "Payment",
            "Account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
            "Destination": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
            "Amount": "1000000",
            "Fee": "30"
        },
        "secret": "s..."
    }],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="sign_for.rs" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let payload = json!({
        "jsonrpc": "2.0",
        "method": "sign_for",
        "params": [{
            "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
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
{% endcode %}
{% endtab %}
{% endtabs %}

### Response Example

```json
{
    "result": {
        "error": "notSupported",
        "error_code": 75,
        "error_message": "Signing is not supported by this server.",
        "request": {
            "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
            "command": "sign_for",
            "secret": "<masked>",
            "tx_json": {
                "Account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
                "TransactionType": "Payment"
            }
        },
        "status": "error"
    }
}
```

### Response Parameters

| Field    | Type   | Description                |
| -------- | ------ | -------------------------- |
| tx\_blob | string | Transaction with signature |
| tx\_json | object | Transaction JSON           |

### Use Cases

* Multi-signature wallets
* Shared custody
* Organizational accounts

### Recommended: Local Multi-Signing

{% tabs %}
{% tab title="xrpl.js" %}
{% code title="local_multisign_xrpl.js" %}
```javascript
const xrpl = require('xrpl');

async function multiSign() {
    const wallet1 = xrpl.Wallet.fromSeed('s...');
    const wallet2 = xrpl.Wallet.fromSeed('s...');
    
    const tx = {
        TransactionType: 'Payment',
        Account: 'rMultiSigAccount...',
        Destination: 'ra5nK24KXen9AHvsdFTKHSANinZseWnPcX',
        Amount: '1000000',
        Fee: '30',
        Sequence: 1,
        SigningPubKey: ''
    };
    
    // Each signer signs
    const sig1 = xrpl.multisign(tx, wallet1);
    const sig2 = xrpl.multisign(tx, wallet2);
    
    // Combine signatures
    const combined = xrpl.combine([sig1, sig2]);
    console.log('Combined:', combined);
}

multiSign();
```
{% endcode %}
{% endtab %}

{% tab title="xrpl-py" %}
{% code title="local_multisign_xrpl_py.py" %}
```python
from xrpl.wallet import Wallet
from xrpl.transaction import multisign

wallet1 = Wallet.from_seed("s...")
wallet2 = Wallet.from_seed("s...")

# See xrpl-py documentation for full multi-sign workflow
```
{% endcode %}
{% endtab %}
{% endtabs %}
