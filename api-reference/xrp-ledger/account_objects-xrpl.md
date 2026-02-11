---
description: >-
  Example code for the account_objects JSON RPC method. Complete guide to using
  account_objects JSON-RPC in the GetBlock Web3 documentation.
---

# account\_objects - XRPL

This method returns the raw ledger format for all objects owned by an account including offers, trust lines, escrows, and payment channels.

## Parameters

| Parameter     | Type          | Required | Description              |
| ------------- | ------------- | -------- | ------------------------ |
| account       | string        | Yes      | Account address          |
| type          | string        | No       | Filter by object type    |
| ledger\_hash  | string        | No       | Ledger hash              |
| ledger\_index | string/number | No       | Ledger index or shortcut |
| limit         | number        | No       | Maximum results          |
| marker        | object        | No       | Pagination marker        |

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
    "method": "account_objects",
    "params": [{
        "account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
        "type": "state"
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
    method: 'account_objects',
    params: [{
        account: 'r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59',
        type: 'state'
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
    "method": "account_objects",
    "params": [{
        "account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
        "type": "state"
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
        "method": "account_objects",
        "params": [{
            "account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
            "type": "state"
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
        "account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
        "account_objects": [
            {
                "Balance": {
                    "currency": "USD",
                    "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
                    "value": "100"
                },
                "LedgerEntryType": "RippleState"
            }
        ],
        "ledger_current_index": 63631836,
        "status": "success"
    }
}
```

## Response Parameters

| Parameter              | Type    | Description    |
| ---------------------- | ------- | -------------- |
| account\_objects       | array   | Ledger objects |
| LedgerEntryType        | string  | Object type    |
| ledger\_current\_index | integer | Current ledger |

## Use Cases

* List all account objects
* Find open offers
* Check escrows

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

async function getObjects() {
    const client = new xrpl.Client('wss://xrplcluster.com');
    await client.connect();
    
    const response = await client.request({
        command: 'account_objects',
        account: 'r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59'
    });
    
    console.log('Objects:', response.result.account_objects.length);
    await client.disconnect();
}

getObjects();
```
{% endcode %}
{% endtab %}

{% tab title="xrpl-py" %}
{% code title="xrpl-py example" %}
```python
from xrpl.clients import JsonRpcClient
from xrpl.models.requests import AccountObjects

client = JsonRpcClient("https://xrp.getblock.io/mainnet/")
request = AccountObjects(account="r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59")
response = client.request(request)
print(response.result)
```
{% endcode %}
{% endtab %}
{% endtabs %}
