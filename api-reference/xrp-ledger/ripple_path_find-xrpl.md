---
description: >-
  Example code for the ripple_path_find JSON RPC method. Complete guide to using
  ripple_path_find JSON-RPC in the GetBlock Web3 documentation.
---

# ripple\_path\_find - XRPL

This method is a simplified version of `path_find` that searches for a path one time and returns results immediately.

## Parameters

| Parameter            | Type          | Required | Description       |
| -------------------- | ------------- | -------- | ----------------- |
| source\_account      | string        | Yes      | Sending account   |
| destination\_account | string        | Yes      | Receiving account |
| destination\_amount  | object        | Yes      | Amount to receive |
| send\_max            | object        | No       | Maximum to send   |
| source\_currencies   | array         | No       | Currencies to use |
| ledger\_hash         | string        | No       | Ledger hash       |
| ledger\_index        | string/number | No       | Ledger index      |

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
    "method": "ripple_path_find",
    "params": [{
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
{% endcode %}
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
    method: 'ripple_path_find',
    params: [{
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
    "method": "ripple_path_find",
    "params": [{
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
        "method": "ripple_path_find",
        "params": [{
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
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code title="response.json" %}
```json
{
    "result": {
        "alternatives": [
            {
                "paths_canonical": [],
                "paths_computed": [
                    [
                        {
                            "currency": "USD",
                            "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
                            "type": 48
                        }
                    ]
                ],
                "source_amount": "83088782"
            }
        ],
        "destination_account": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
        "destination_amount": {
            "currency": "USD",
            "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
            "value": "100"
        },
        "destination_currencies": [
            "USD",
            "XRP"
        ],
        "full_reply": true,
        "source_account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
        "status": "success"
    }
}
```
{% endcode %}

## Returns

| Field                   | Type   | Description          |
| ----------------------- | ------ | -------------------- |
| alternatives            | array  | Payment paths found  |
| destination\_account    | string | Destination          |
| destination\_currencies | array  | Available currencies |

## Use Cases

* One-time path queries
* Payment validation
* Quote generation

## SDK Integration

### xrpl.js

{% code title="xrpl-path-find.js" %}
```javascript
const xrpl = require('xrpl');

async function ripplePathFind() {
    const client = new xrpl.Client('wss://xrplcluster.com');
    await client.connect();
    
    const response = await client.request({
        command: 'ripple_path_find',
        source_account: 'rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH',
        destination_account: 'ra5nK24KXen9AHvsdFTKHSANinZseWnPcX',
        destination_amount: {
            currency: 'USD',
            issuer: 'rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B',
            value: '100'
        }
    });
    
    console.log('Alternatives:', response.result.alternatives);
    await client.disconnect();
}

ripplePathFind();
```
{% endcode %}
