---
description: >-
  Example code for the path_find JSON RPC method. Complete guide to using
  path_find JSON-RPC in the GetBlock Web3 documentation.
---

# path\_find - XRPL

This method searches for a path along which a payment can be made, and periodically sends updates when the path changes.

## Parameters

| Parameter            | Type   | Required | Description                    |
| -------------------- | ------ | -------- | ------------------------------ |
| subcommand           | string | Yes      | "create", "close", or "status" |
| source\_account      | string | Yes      | Sending account                |
| destination\_account | string | Yes      | Receiving account              |
| destination\_amount  | object | Yes      | Amount to receive              |
| send\_max            | object | No       | Maximum to send                |
| source\_currencies   | array  | No       | Currencies to use              |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://xrp.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "path_find",
    "params": [{
        "subcommand": "create",
        "source_account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
        "destination_account": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
        "destination_amount": {
            "currency": "USD",
            "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
            "value": "100"
        }
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
    method: 'path_find',
    params: [{
        subcommand: 'create',
        source_account: 'rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH',
        destination_account: 'ra5nK24KXen9AHvsdFTKHSANinZseWnPcX',
        destination_amount: {
            currency: 'USD',
            issuer: 'rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B',
            value: '100'
        }
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
    "method": "path_find",
    "params": [{
        "subcommand": "create",
        "source_account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
        "destination_account": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
        "destination_amount": {
            "currency": "USD",
            "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
            "value": "100"
        }
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
        "method": "path_find",
        "params": [{
            "subcommand": "create",
            "source_account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
            "destination_account": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
            "destination_amount": {
                "currency": "USD",
                "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
                "value": "100"
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
{% endtab %}
{% endtabs %}

## Response Example

{% code title="Response Example (JSON)" %}
```json
{
    "result": {
        "alternatives": [
            {
                "paths_computed": [
                    [{"currency": "USD", "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B"}]
                ],
                "source_amount": "1000000"
            }
        ],
        "destination_account": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
        "destination_amount": {
            "currency": "USD",
            "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
            "value": "100"
        },
        "status": "success"
    }
}
```
{% endcode %}

## Returns

| Field                | Type   | Description         |
| -------------------- | ------ | ------------------- |
| alternatives         | array  | Payment paths found |
| destination\_account | string | Destination         |
| destination\_amount  | object | Amount              |

## Use Cases

* Cross-currency payments
* Find best exchange rates
* Payment routing

## SDK Integration

{% tabs %}
{% tab title="First Tab" %}
```javascript
const xrpl = require('xrpl');

async function findPath() {
    const client = new xrpl.Client('wss://xrplcluster.com');
    await client.connect();
    
    const response = await client.request({
        command: 'path_find',
        subcommand: 'create',
        source_account: 'rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH',
        destination_account: 'ra5nK24KXen9AHvsdFTKHSANinZseWnPcX',
        destination_amount: {
            currency: 'USD',
            issuer: 'rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B',
            value: '100'
        }
    });
    
    console.log('Paths:', response.result.alternatives);
    await client.disconnect();
}

findPath();
```
{% endtab %}
{% endtabs %}
