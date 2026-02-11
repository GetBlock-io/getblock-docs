---
description: >-
  Example code for the deposit_authorized JSON RPC method. Complete guide to
  using deposit_authorized JSON-RPC in the GetBlock Web3 documentation.
---

# deposit\_authorized - XRPL

The `deposit_authorized` method indicates whether one account is authorized to send payments directly to another.

## Parameters

| Parameter            | Type          | Required | Description       |
| -------------------- | ------------- | -------- | ----------------- |
| source\_account      | string        | Yes      | Sending account   |
| destination\_account | string        | Yes      | Receiving account |
| ledger\_hash         | string        | No       | Ledger hash       |
| ledger\_index        | string/number | No       | Ledger index      |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://xrp.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "deposit_authorized",
    "params": [{
        "source_account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
        "destination_account": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX"
    }],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="index.js" %}
```javascript
const axios = require('axios');

const url = 'https://xrp.getblock.io/mainnet/';
const headers = {
    'x-api-key': 'YOUR-API-KEY',
    'Content-Type': 'application/json'
};

const payload = {
    jsonrpc: '2.0',
    method: 'deposit_authorized',
    params: [{
        source_account: 'rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH',
        destination_account: 'ra5nK24KXen9AHvsdFTKHSANinZseWnPcX'
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
    "method": "deposit_authorized",
    "params": [{
        "source_account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
        "destination_account": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX"
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
        "method": "deposit_authorized",
        "params": [{
            "source_account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
            "destination_account": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX"
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
        "deposit_authorized": true,
        "destination_account": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
        "ledger_current_index": 102053961,
        "source_account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
        "status": "success",
        "validated": false
    }
}
```

## Response Parameters

| Field                | Type    | Description                   |
| -------------------- | ------- | ----------------------------- |
| deposit\_authorized  | boolean | Whether deposit is authorized |
| source\_account      | string  | Source account                |
| destination\_account | string  | Destination account           |

## Use Cases

* Pre-validate payments
* Check deposit permissions
* Handle deposit preauthorization

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

async function checkAuth() {
    const client = new xrpl.Client('wss://xrplcluster.com');
    await client.connect();
    
    const response = await client.request({
        command: 'deposit_authorized',
        source_account: 'rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH',
        destination_account: 'ra5nK24KXen9AHvsdFTKHSANinZseWnPcX'
    });
    
    console.log('Authorized:', response.result.deposit_authorized);
    await client.disconnect();
}

checkAuth();
```
{% endcode %}
{% endtab %}

{% tab title="xrpl-py" %}
{% code title="xrpl-py example" %}
```python
from xrpl.clients import JsonRpcClient
from xrpl.models.requests import DepositAuthorized

client = JsonRpcClient("https://xrp.getblock.io/mainnet/")
request = DepositAuthorized(
    source_account="rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
    destination_account="ra5nK24KXen9AHvsdFTKHSANinZseWnPcX"
)
response = client.request(request)
print(response.result)
```
{% endcode %}
{% endtab %}
{% endtabs %}
