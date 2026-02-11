---
description: >-
  Example code for the account_tx JSON RPC method. Complete guide to using
  account_tx JSON-RPC in the GetBlock Web3 documentation.
---

# account\_tx - XRPL

This method retrieves a list of transactions that involved the specified account.

## Parameters

| Parameter          | Type    | Required | Description          |
| ------------------ | ------- | -------- | -------------------- |
| account            | string  | Yes      | Account address      |
| ledger\_index\_min | number  | No       | Earliest ledger      |
| ledger\_index\_max | number  | No       | Latest ledger        |
| binary             | boolean | No       | Return binary format |
| forward            | boolean | No       | Sort oldest first    |
| limit              | number  | No       | Maximum results      |
| marker             | object  | No       | Pagination marker    |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://xrp.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "account_tx",
    "params": [{
        "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
        "limit": 10
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
    method: 'account_tx',
    params: [{
        account: 'rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH',
        limit: 10
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
    "method": "account_tx",
    "params": [{
        "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
        "limit": 10
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
        "method": "account_tx",
        "params": [{
            "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
            "limit": 10
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
        "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
        "ledger_index_max": 102053170,
        "ledger_index_min": 100807399,
        "limit": 10,
        "status": "success",
        "transactions": [],
        "validated": true
    }
}
```

## Response Parameters

| Parameter          | Type    | Description          |
| ------------------ | ------- | -------------------- |
| transactions       | array   | Transaction list     |
| tx                 | object  | Transaction data     |
| meta               | object  | Transaction metadata |
| ledger\_index\_min | integer | Earliest searched    |
| ledger\_index\_max | integer | Latest searched      |

## Use Cases

* Build transaction history
* Track payments
* Monitor account activity

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

async function getTx() {
    const client = new xrpl.Client('wss://xrplcluster.com');
    await client.connect();
    
    const response = await client.request({
        command: 'account_tx',
        account: 'rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH',
        limit: 10
    });
    
    console.log('Transactions:', response.result.transactions.length);
    await client.disconnect();
}

getTx();
```
{% endcode %}
{% endtab %}

{% tab title="xrpl-py" %}
{% code title="xrpl-py example" %}
```python
from xrpl.clients import JsonRpcClient
from xrpl.models.requests import AccountTx

client = JsonRpcClient("https://xrp.getblock.io/mainnet/")
request = AccountTx(account="rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH", limit=10)
response = client.request(request)
print(response.result)
```
{% endcode %}
{% endtab %}
{% endtabs %}
