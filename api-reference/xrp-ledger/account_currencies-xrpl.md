---
description: >-
  Example code for the account_currencies JSON RPC method. Complete guide to
  using account_currencies JSON-RPC in the GetBlock Web3 documentation.
---

# account\_currencies - XRPL

This method retrieves a list of currencies that an account can send or receive, based on its trust lines.

## Parameters

| Parameter     | Type          | Required | Description              |
| ------------- | ------------- | -------- | ------------------------ |
| account       | string        | Yes      | Account address          |
| ledger\_hash  | string        | No       | Ledger hash              |
| ledger\_index | string/number | No       | Ledger index or shortcut |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://xrp.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "account_currencies",
    "params": [{
        "account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
        "ledger_index": "validated"
    }],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const url = 'https://xrp.getblock.io/mainnet/';
const headers = {
    'x-api-key': 'YOUR-API-KEY',
    'Content-Type': 'application/json'
};

const payload = {
    jsonrpc: '2.0',
    method: 'account_currencies',
    params: [{
        account: 'r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59',
        ledger_index: 'validated'
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
{% code title="example.py" %}
```python
import requests

url = "https://xrp.getblock.io/mainnet/"
headers = {
    "x-api-key": "YOUR-API-KEY",
    "Content-Type": "application/json"
}

payload = {
    "jsonrpc": "2.0",
    "method": "account_currencies",
    "params": [{
        "account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
        "ledger_index": "validated"
    }],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, json=payload)
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
        "method": "account_currencies",
        "params": [{
            "account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
            "ledger_index": "validated"
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
        "ledger_index": 63631836,
        "receive_currencies": ["USD", "EUR", "BTC"],
        "send_currencies": ["USD", "EUR"],
        "status": "success",
        "validated": true
    }
}
```
{% endcode %}

## Response Parameters

| Parameter           | Type    | Description           |
| ------------------- | ------- | --------------------- |
| receive\_currencies | array   | Receivable currencies |
| send\_currencies    | array   | Sendable currencies   |
| ledger\_index       | integer | Ledger index used     |

## Use Cases

* Check supported currencies
* Build currency selection UIs
* Validate payment paths

## Error Handling

| Error Code    | Description        |
| ------------- | ------------------ |
| actNotFound   | Account not found  |
| invalidParams | Invalid parameters |

## SDK Integration

{% tabs %}
{% tab title="xrpl.js" %}
{% code title="xrpl.js example" %}
```javascript
const xrpl = require('xrpl');

async function getCurrencies() {
    const client = new xrpl.Client('wss://xrplcluster.com');
    await client.connect();
    
    const response = await client.request({
        command: 'account_currencies',
        account: 'r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59'
    });
    
    console.log('Can receive:', response.result.receive_currencies);
    await client.disconnect();
}

getCurrencies();
```
{% endcode %}
{% endtab %}

{% tab title="xrpl-py" %}
{% code title="xrpl-py example" %}
```python
from xrpl.clients import JsonRpcClient
from xrpl.models.requests import AccountCurrencies

client = JsonRpcClient("https://xrp.getblock.io/mainnet/")
request = AccountCurrencies(account="r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59")
response = client.request(request)
print(response.result)
```
{% endcode %}
{% endtab %}
{% endtabs %}
