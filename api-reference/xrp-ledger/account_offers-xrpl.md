---
description: >-
  Example code for the account_offers JSON RPC method. Complete guide to using
  account_offers JSON-RPC in the GetBlock Web3 documentation.
---

# account\_offers - XRPL

This method retrieves a list of offers made by a given account that are outstanding as of a particular ledger version.

## Parameters

| Parameter     | Type          | Required | Description              |
| ------------- | ------------- | -------- | ------------------------ |
| account       | string        | Yes      | Account address          |
| ledger\_hash  | string        | No       | Ledger hash              |
| ledger\_index | string/number | No       | Ledger index or shortcut |
| limit         | number        | No       | Maximum results          |
| marker        | object        | No       | Pagination marker        |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://xrp.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "account_offers",
    "params": [{
        "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH"
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

const payload = {
    jsonrpc: '2.0',
    method: 'account_offers',
    params: [{
        account: 'rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH'
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

payload = {
    "jsonrpc": "2.0",
    "method": "account_offers",
    "params": [{
        "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH"
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
        "method": "account_offers",
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
{% endtab %}
{% endtabs %}

## Response Example

{% code title="response.json" %}
```json
{
    "result": {
        "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
        "ledger_current_index": 63631836,
        "offers": [],
        "status": "success"
    }
}
```
{% endcode %}

## Response Parameters

| Parameter              | Type    | Description       |
| ---------------------- | ------- | ----------------- |
| offers                 | array   | Open offers       |
| taker\_gets            | object  | Amount to receive |
| taker\_pays            | object  | Amount to pay     |
| seq                    | integer | Offer sequence    |
| ledger\_current\_index | integer | Current ledger    |

## Use Cases

* View open orders
* Cancel specific offers
* Monitor trading activity

## Error Handling

| Error Code    | Description        |
| ------------- | ------------------ |
| actNotFound   | Account not found  |
| invalidParams | Invalid parameters |

## SDK Integration

{% tabs %}
{% tab title="xrpl.js" %}
```javascript
const xrpl = require('xrpl');

async function getOffers() {
    const client = new xrpl.Client('wss://xrplcluster.com');
    await client.connect();
    
    const response = await client.request({
        command: 'account_offers',
        account: 'rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH'
    });
    
    console.log('Offers:', response.result.offers);
    await client.disconnect();
}

getOffers();
```
{% endtab %}

{% tab title="xrpl-py" %}
```python
from xrpl.clients import JsonRpcClient
from xrpl.models.requests import AccountOffers

client = JsonRpcClient("https://xrp.getblock.io/mainnet/")
request = AccountOffers(account="rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH")
response = client.request(request)
print(response.result)
```
{% endtab %}
{% endtabs %}
