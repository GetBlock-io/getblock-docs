---
description: >-
  Example code for the transaction_entry JSON RPC method. Complete guide to
  using transaction_entry JSON-RPC in the GetBlock Web3 documentation.
---

# transaction\_entry - XRPL

This method retrieves information on a single transaction from a specific ledger version. The `tx` Method is recommended instead, as it searches all ledgers.

## Parameters

| Parameter     | Type          | Required | Description      |
| ------------- | ------------- | -------- | ---------------- |
| tx\_hash      | string        | Yes      | Transaction hash |
| ledger\_hash  | string        | No       | Ledger hash      |
| ledger\_index | string/number | No       | Ledger index     |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://xrp.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "transaction_entry",
    "params": [{
        "tx_hash": "C53ECF838647FA5A4C780377025FEC7999AB4182590510CA461444B207AB74A9",
        "ledger_index": 63632029
    }],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="JavaScript (Axios)" %}
```javascript
const axios = require('axios');

const url = 'https://xrp.getblock.io/mainnet/';
const headers = {
    'x-api-key': 'YOUR-API-KEY',
    'Content-Type': 'application/json'
};

const payload = {
    jsonrpc: '2.0',
    method: 'transaction_entry',
    params: [{
        tx_hash: 'C53ECF838647FA5A4C780377025FEC7999AB4182590510CA461444B207AB74A9',
        ledger_index: 63632029
    }],
    id: 'getblock.io'
};

axios.post(url, payload, { headers })
    .then(response => console.log(response.data))
    .catch(error => console.error(error));
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="Python" %}
```python
import requests

url = "https://xrp.getblock.io/mainnet/"
headers = {
    "x-api-key": "YOUR-API-KEY",
    "Content-Type": "application/json"
}

payload = {
    "jsonrpc": "2.0",
    "method": "transaction_entry",
    "params": [{
        "tx_hash": "C53ECF838647FA5A4C780377025FEC7999AB4182590510CA461444B207AB74A9",
        "ledger_index": 63632029
    }],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="Rust" overflow="wrap" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let payload = json!({
        "jsonrpc": "2.0",
        "method": "transaction_entry",
        "params": [{
            "tx_hash": "C53ECF838647FA5A4C780377025FEC7999AB4182590510CA461444B207AB74A9",
            "ledger_index": 63632029
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

```json
{
    "result": {
        "ledger_index": 63632029,
        "metadata": {
            "TransactionResult": "tesSUCCESS"
        },
        "status": "success",
        "tx_json": {
            "Account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
            "TransactionType": "Payment"
        }
    }
}
```

## Response Parameters

| Parameter     | Type    | Description          |
| ------------- | ------- | -------------------- |
| tx\_json      | object  | Transaction data     |
| metadata      | object  | Transaction result   |
| ledger\_index | integer | Ledger containing tx |

## Use Cases

* Get transaction from specific ledger
* Historical analysis
* Ledger auditing

## Error Handling

<details>

<summary>txnNotFound</summary>

Transaction not found

</details>

<details>

<summary>lgrNotFound</summary>

Ledger not found

</details>

## SDK Integration

{% tabs %}
{% tab title="xrpl.js" %}
{% code title="xrpl.js" %}
```javascript
const xrpl = require('xrpl');

async function getTxEntry() {
    const client = new xrpl.Client('wss://xrplcluster.com');
    await client.connect();
    
    const response = await client.request({
        command: 'transaction_entry',
        tx_hash: 'C53ECF838647FA5A4C780377025FEC7999AB4182590510CA461444B207AB74A9',
        ledger_index: 63632029
    });
    
    console.log(response.result);
    await client.disconnect();
}

getTxEntry();
```
{% endcode %}
{% endtab %}

{% tab title="xrpl.py" %}
{% code title="xrpl-py" %}
```python
from xrpl.clients import JsonRpcClient
from xrpl.models.requests import TransactionEntry

client = JsonRpcClient("https://xrp.getblock.io/mainnet/")
request = TransactionEntry(
    tx_hash="C53ECF838647FA5A4C780377025FEC7999AB4182590510CA461444B207AB74A9",
    ledger_index=63632029
)
response = client.request(request)
print(response.result)
```
{% endcode %}
{% endtab %}
{% endtabs %}
