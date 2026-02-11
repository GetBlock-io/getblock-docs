---
description: >-
  Example code for the account_channels JSON RPC method. Complete guide to using
  account_channels JSON-RPC in the GetBlock Web3 documentation.
---

# account\_channels - XRPL

Returns payment channels where the account is the source on XRP Ledger.

## Parameters

| Parameter            | Type          | Required | Description              |
| -------------------- | ------------- | -------- | ------------------------ |
| account              | string        | Yes      | Account address          |
| destination\_account | string        | No       | Filter by destination    |
| ledger\_hash         | string        | No       | Ledger hash              |
| ledger\_index        | string/number | No       | Ledger index or shortcut |
| limit                | number        | No       | Maximum results          |
| marker               | object        | No       | Pagination marker        |

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
    "method": "account_channels",
    "params": [{
        "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH"
    }],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="request.js" %}
```javascript
const axios = require('axios');

const url = 'https://xrp.getblock.io/mainnet/';
const headers = {
    'x-api-key': 'YOUR-API-KEY',
    'Content-Type': 'application/json'
};

const payload = {
    jsonrpc: '2.0',
    method: 'account_channels',
    params: [{
        account: 'rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH'
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
{% code title="request.py" %}
```python
import requests

url = "https://xrp.getblock.io/mainnet/"
headers = {
    "x-api-key": "YOUR-API-KEY",
    "Content-Type": "application/json"
}

payload = {
    "jsonrpc": "2.0",
    "method": "account_channels",
    "params": [{
        "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH"
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
        "method": "account_channels",
        "params": [{
            "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH"
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
        "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
        "channels": [],
        "ledger_current_index": 102024783,
        "status": "success",
        "validated": false
    }
}
```
{% endcode %}

## Response Parameters

| Parameter   | Type   | Description        |
| ----------- | ------ | ------------------ |
| channels    | array  | Payment channels   |
| channel\_id | string | Channel identifier |
| amount      | string | Total allocated    |
| balance     | string | Amount paid out    |

| Field                  | Type    | Description          |
| ---------------------- | ------- | -------------------- |
| account                | string  | Source account       |
| channels               | array   | Payment channel list |
| ledger\_current\_index | integer | Current ledger index |

## Use Cases

* List payment channels
* Monitor channel balances
* Track streaming payments

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

async function getChannels() {
    const client = new xrpl.Client('wss://xrplcluster.com');
    await client.connect();
    
    const response = await client.request({
        command: 'account_channels',
        account: 'rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH'
    });
    
    console.log('Channels:', response.result.channels);
    await client.disconnect();
}

getChannels();
```
{% endcode %}
{% endtab %}

{% tab title="xrpl-py" %}
{% code title="xrpl-py example" %}
```python
from xrpl.clients import JsonRpcClient
from xrpl.models.requests import AccountChannels

client = JsonRpcClient("https://xrp.getblock.io/mainnet/")
request = AccountChannels(account="rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH")
response = client.request(request)
print(response.result)
```
{% endcode %}
{% endtab %}
{% endtabs %}
