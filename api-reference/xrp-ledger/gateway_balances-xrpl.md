---
description: >-
  Example code for the gateway_balances JSON RPC method. Complete guide to using
  gateway_balances JSON-RPC in the GetBlock Web3 documentation.
---

# gateway\_balances - XRPL

Returns gateway balances on XRP Ledger.

## Parameters

| Parameter     | Type          | Required | Description               |
| ------------- | ------------- | -------- | ------------------------- |
| account       | string        | Yes      | Gateway account           |
| strict        | boolean       | No       | Fail if not valid address |
| hotwallet     | string/array  | No       | Operational wallets       |
| ledger\_hash  | string        | No       | Ledger hash               |
| ledger\_index | string/number | No       | Ledger index              |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="Request (cURL)" %}
```bash
curl --location --request POST 'https://xrp.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "gateway_balances",
    "params": [{
        "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
        "strict": true
    }],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="Request (JavaScript - Axios)" %}
```javascript
const axios = require('axios');

const url = 'https://xrp.getblock.io/mainnet/';
const headers = {
    'x-api-key': 'YOUR-API-KEY',
    'Content-Type': 'application/json'
};

const payload = {
    jsonrpc: '2.0',
    method: 'gateway_balances',
    params: [{
        account: 'rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH',
        strict: true
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
{% code title="Request (Python)" %}
```python
import requests

url = "https://xrp.getblock.io/mainnet/"
headers = {
    "x-api-key": "YOUR-API-KEY",
    "Content-Type": "application/json"
}

payload = {
    "jsonrpc": "2.0",
    "method": "gateway_balances",
    "params": [{
        "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
        "strict": True
    }],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="Request (Rust)" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let payload = json!({
        "jsonrpc": "2.0",
        "method": "gateway_balances",
        "params": [{
            "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
            "strict": true
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

{% code title="Response (JSON)" %}
```json
{
    "result": {
        "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
        "ledger_current_index": 63632030,
        "obligations": {
            "USD": "100000"
        },
        "status": "success"
    }
}
```
{% endcode %}

## Response Parameters

| Field       | Type   | Description        |
| ----------- | ------ | ------------------ |
| account     | string | Gateway account    |
| obligations | object | Total obligations  |
| balances    | object | Hotwallet balances |
| assets      | object | Assets held        |

## Use Cases

* Gateway accounting
* Track issued currencies
* Monitor obligations

## Error Handling

<details>

<summary>actNotFound</summary>

Account not found

</details>

<details>

<summary>invalidParams</summary>

Invalid parameters

</details>

## SDK Integration

{% tabs %}
{% tab title="xrpl.js" %}
{% code title="xrpl.js example" %}
```javascript
const xrpl = require('xrpl');

async function getGatewayBalances() {
    const client = new xrpl.Client('wss://xrplcluster.com');
    await client.connect();
    
    const response = await client.request({
        command: 'gateway_balances',
        account: 'rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH'
    });
    
    console.log('Obligations:', response.result.obligations);
    await client.disconnect();
}

getGatewayBalances();
```
{% endcode %}
{% endtab %}

{% tab title="xrpl-py" %}
{% code title="xrpl-py example" %}
```python
from xrpl.clients import JsonRpcClient
from xrpl.models.requests import GatewayBalances

client = JsonRpcClient("https://xrp.getblock.io/mainnet/")
request = GatewayBalances(account="rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH")
response = client.request(request)
print(response.result)
```
{% endcode %}
{% endtab %}
{% endtabs %}
