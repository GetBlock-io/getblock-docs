---
description: >-
  Example code for the unsubscribe JSON RPC method. Complete guide to using
  unsubscribe JSON-RPC in the GetBlock Web3 documentation.
---

# unsubscribe - XRPL

This method tells the server to stop sending messages for a particular subscription.

## Parameters

| Parameter          | Type  | Required | Description                  |
| ------------------ | ----- | -------- | ---------------------------- |
| streams            | array | No       | Streams to unsubscribe       |
| accounts           | array | No       | Accounts to stop watching    |
| accounts\_proposed | array | No       | Proposed tx accounts         |
| books              | array | No       | Order books to stop watching |

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
    "method": "unsubscribe",
    "params": [{
        "streams": ["ledger"]
    }],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="unsubscribe.js" %}
```javascript
const axios = require('axios');

const url = 'https://xrp.getblock.io/mainnet/';
const headers = {
    'x-api-key': 'YOUR-API-KEY',
    'Content-Type': 'application/json'
};

const payload = {
    jsonrpc: '2.0',
    method: 'unsubscribe',
    params: [{
        streams: ['ledger']
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
{% code title="unsubscribe.py" %}
```python
import requests

url = "https://xrp.getblock.io/mainnet/"
headers = {
    "x-api-key": "YOUR-API-KEY",
    "Content-Type": "application/json"
}

payload = {
    "jsonrpc": "2.0",
    "method": "unsubscribe",
    "params": [{
        "streams": ["ledger"]
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
        "method": "unsubscribe",
        "params": [{
            "streams": ["ledger"]
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
        "status": "success"
    }
}
```

## Response Parameters

| Parameter | Type   | Description        |
| --------- | ------ | ------------------ |
| status    | string | Unsubscribe status |

## Use Cases

* Stop receiving updates
* Clean up connections
* Resource management

## Error Handling

| Error Code    | Description        |
| ------------- | ------------------ |
| invalidParams | Invalid parameters |

## SDK Integration

{% tabs %}
{% tab title="xrpl.js" %}
{% code title="xrpl.js (Node.js)" %}
```javascript
const xrpl = require('xrpl');

async function unsubscribe() {
    const client = new xrpl.Client('wss://xrplcluster.com');
    await client.connect();
    
    await client.request({
        command: 'unsubscribe',
        streams: ['ledger']
    });
    
    console.log('Unsubscribed');
    await client.disconnect();
}

unsubscribe();
```
{% endcode %}
{% endtab %}

{% tab title="xrpl-py" %}
{% code title="xrpl-py (Python)" %}
```python
from xrpl.clients import JsonRpcClient
from xrpl.models.requests import Unsubscribe

client = JsonRpcClient("https://xrp.getblock.io/mainnet/")
request = Unsubscribe(streams=["ledger"])
response = client.request(request)
print(response.result)
```
{% endcode %}
{% endtab %}
{% endtabs %}
