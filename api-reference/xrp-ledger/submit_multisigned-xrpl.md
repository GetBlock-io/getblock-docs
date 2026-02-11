---
description: >-
  Example code for the submit_multisigned JSON RPC method. Complete guide to
  using submit_multisigned JSON-RPC in the GetBlock Web3 documentation.
---

# submit\_multisigned - XRPL

This method applies a multi-signed transaction and sends it to the network.

## Parameters

| Parameter  | Type    | Required | Description                   |
| ---------- | ------- | -------- | ----------------------------- |
| tx\_json   | object  | Yes      | Multi-signed transaction JSON |
| fail\_hard | boolean | No       | Fail if local error           |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="curl" %}
```bash
curl --location --request POST 'https://xrp.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "submit_multisigned",
    "params": [{
        "tx_json": {
            "Account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
            "TransactionType": "Payment",
            "Destination": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
            "Amount": "1000000",
            "Fee": "30",
            "Signers": []
        }
    }],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="submit_multisigned.js" %}
```javascript
const axios = require('axios');

const url = 'https://xrp.getblock.io/mainnet/';
const headers = {
    'x-api-key': 'YOUR-API-KEY',
    'Content-Type': 'application/json'
};

const payload = {
    jsonrpc: '2.0',
    method: 'submit_multisigned',
    params: [{
        tx_json: {
            Account: 'rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH',
            TransactionType: 'Payment',
            Destination: 'ra5nK24KXen9AHvsdFTKHSANinZseWnPcX',
            Amount: '1000000',
            Fee: '30',
            Signers: []
        }
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
{% code title="submit_multisigned.py" %}
```python
import requests

url = "https://xrp.getblock.io/mainnet/"
headers = {
    "x-api-key": "YOUR-API-KEY",
    "Content-Type": "application/json"
}

payload = {
    "jsonrpc": "2.0",
    "method": "submit_multisigned",
    "params": [{
        "tx_json": {
            "Account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
            "TransactionType": "Payment",
            "Destination": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
            "Amount": "1000000",
            "Fee": "30",
            "Signers": []
        }
    }],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="submit_multisigned.rs" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let payload = json!({
        "jsonrpc": "2.0",
        "method": "submit_multisigned",
        "params": [{
            "tx_json": {
                "Account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
                "TransactionType": "Payment",
                "Signers": []
            }
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

## Response Example

{% code title="response.json" %}
```json
{
    "result": {
        "engine_result": "tesSUCCESS",
        "engine_result_code": 0,
        "engine_result_message": "The transaction was applied.",
        "status": "success",
        "tx_json": {
            "Account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
            "TransactionType": "Payment"
        }
    }
}
```
{% endcode %}

## Response Parameters

| Field                | Type    | Description        |
| -------------------- | ------- | ------------------ |
| engine\_result       | string  | Transaction result |
| engine\_result\_code | integer | Result code        |
| tx\_blob             | string  | Transaction hex    |
| tx\_json             | object  | Transaction JSON   |
| signer               | array   | Signer list        |

## Use Cases

* Multi-signature accounts
* Shared custody
* Organizational accounts

## Error Handling

| Error Code             | Description           |
| ---------------------- | --------------------- |
| temINVALID             | Invalid transaction   |
| tefNOT\_MULTI\_SIGNING | Not multi-sig enabled |

## SDK Integration

{% tabs %}
{% tab title="xrpl.js" %}
{% code title="xrpl.js (example)" %}
```javascript
const xrpl = require('xrpl');

// Multi-signing requires collecting signatures from multiple wallets
// See xrpl.js documentation for full multi-sign workflow
```
{% endcode %}
{% endtab %}

{% tab title="xrpl.py" %}
{% code title="xrpl-py (example)" %}
```python
from xrpl.clients import JsonRpcClient
from xrpl.models.requests import SubmitMultisigned

client = JsonRpcClient("https://xrp.getblock.io/mainnet/")
# Collect signatures and submit
```
{% endcode %}
{% endtab %}
{% endtabs %}
