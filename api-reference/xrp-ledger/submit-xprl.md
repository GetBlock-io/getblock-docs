---
description: >-
  Example code for the submit JSON RPC method. Complete guide to using submit
  JSON-RPC in the GetBlock Web3 documentation.
---

# submit - XPRL

Submits a signed transaction to XRP Ledger.

## Parameters

| Parameter  | Type    | Required | Description               |
| ---------- | ------- | -------- | ------------------------- |
| tx\_blob   | string  | Yes      | Signed transaction in hex |
| fail\_hard | boolean | No       | Fail if local error       |

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
    "method": "submit",
    "params": [{
        "tx_blob": "1200002280000000240000000361D4838D7EA4C6800000000000000000000000000055534400000000004B4E9C06F24296074F7BC48F92A97916C6DC5EA968400000000000000A732103AB40A0490F9B7ED8DF29D246BF2D6269820A0EE7742ACDD457BEA7C7D0931EDB7446304402207EB0AE8B..."
    }],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="submit.js" %}
```javascript
const axios = require('axios');

const url = 'https://xrp.getblock.io/mainnet/';
const headers = {
    'x-api-key': 'YOUR-API-KEY',
    'Content-Type': 'application/json'
};

const payload = {
    jsonrpc: '2.0',
    method: 'submit',
    params: [{
        tx_blob: '1200002280000000240000000361D4838D7EA4C6800000000000000000000000000055534400000000004B4E9C06F24296074F7BC48F92A97916C6DC5EA968400000000000000A732103AB40A0490F9B7ED8DF29D246BF2D6269820A0EE7742ACDD457BEA7C7D0931EDB7446304402207EB0AE8B...'
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
{% code title="submit.py" %}
```python
import requests

url = "https://xrp.getblock.io/mainnet/"
headers = {
    "x-api-key": "YOUR-API-KEY",
    "Content-Type": "application/json"
}

payload = {
    "jsonrpc": "2.0",
    "method": "submit",
    "params": [{
        "tx_blob": "1200002280000000240000000361D4838D7EA4C6800000000000000000000000000055534400000000004B4E9C06F24296074F7BC48F92A97916C6DC5EA968400000000000000A732103AB40A0490F9B7ED8DF29D246BF2D6269820A0EE7742ACDD457BEA7C7D0931EDB7446304402207EB0AE8B..."
    }],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="submit.rs" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let payload = json!({
        "jsonrpc": "2.0",
        "method": "submit",
        "params": [{
            "tx_blob": "1200002280000000..."
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
        "tx_blob": "1200002280000000...",
        "tx_json": {
            "Account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
            "TransactionType": "Payment"
        }
    }
}
```
{% endcode %}

## Response Parameters

| Field                   | Type   | Description             |
| ----------------------- | ------ | ----------------------- |
| engine\_result          | string | Transaction result code |
| engine\_result\_code    | number | Numeric result code     |
| engine\_result\_message | string | Human-readable message  |
| tx\_blob                | string | Transaction hex         |
| tx\_json                | object | Transaction as JSON     |

## Use Cases

* Submit payments
* Create offers
* Set trust lines

## Error Handling

| Error Code   | Description           |
| ------------ | --------------------- |
| temMALFORMED | Malformed transaction |
| tefPAST\_SEQ | Sequence too low      |
| terPRE\_SEQ  | Sequence too high     |

## SDK Integration

{% tabs %}
{% tab title="xrpl.js" %}
{% code title="xrpl.js (example)" %}
```javascript
const xrpl = require('xrpl');

async function submitTx() {
    const client = new xrpl.Client('wss://xrplcluster.com');
    await client.connect();
    
    const wallet = xrpl.Wallet.fromSeed('s...');
    const payment = {
        TransactionType: 'Payment',
        Account: wallet.address,
        Destination: 'ra5nK24KXen9AHvsdFTKHSANinZseWnPcX',
        Amount: '1000000'
    };
    
    const result = await client.submitAndWait(payment, { wallet });
    console.log('Result:', result.result.meta.TransactionResult);
    
    await client.disconnect();
}

submitTx();
```
{% endcode %}
{% endtab %}

{% tab title="xrpl-py" %}
{% code title="xrpl-py (snippet)" %}
```python
from xrpl.clients import JsonRpcClient
from xrpl.models.transactions import Payment
from xrpl.wallet import generate_faucet_wallet

client = JsonRpcClient("https://xrp.getblock.io/mainnet/")
# Note: Sign transactions locally before submitting
```
{% endcode %}
{% endtab %}
{% endtabs %}
