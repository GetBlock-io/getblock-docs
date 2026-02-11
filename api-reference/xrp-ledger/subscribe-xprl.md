---
description: >-
  Example code for the subscribe JSON RPC method. Complete guide to using
  subscribe JSON-RPC in the GetBlock Web3 documentation.
---

# subscribe - XPRL

This method requests periodic notifications from the server when certain events happen.

## Parameters

| Parameter          | Type  | Required | Description               |
| ------------------ | ----- | -------- | ------------------------- |
| streams            | array | No       | Streams to subscribe      |
| accounts           | array | No       | Accounts to watch         |
| accounts\_proposed | array | No       | Accounts for proposed txs |
| books              | array | No       | Order books to watch      |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://xrp.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "subscribe",
    "params": [{
        "streams": ["ledger"]
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
    method: 'subscribe',
    params: [{
        streams: ['ledger']
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
    "method": "subscribe",
    "params": [{
        "streams": ["ledger"]
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
        "method": "subscribe",
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
{% endtab %}
{% endtabs %}

## Response Example

```json
{
    "result": {
        "fee_base": 10,
        "fee_ref": 10,
        "ledger_hash": "3A0B41C31B13436610C5961E2C914529CAD3CCC4A7859ED6AABFCD0700847BC1",
        "ledger_index": 63632030,
        "ledger_time": 674581032,
        "reserve_base": 10000000,
        "reserve_inc": 2000000,
        "status": "success"
    }
}
```

## Response Parameters

| Parameter     | Type   | Description         |
| ------------- | ------ | ------------------- |
| ledger\_index | number | Current ledger      |
| status        | string | Subscription status |

## Use Cases

* Real-time ledger updates
* Monitor account activity
* Track order book changes

## Error Handling

| Error Code    | Description        |
| ------------- | ------------------ |
| invalidParams | Invalid parameters |

## SDK Integration

{% tabs %}
{% tab title="xrpl.js" %}
```javascript
const xrpl = require('xrpl');

async function subscribe() {
    const client = new xrpl.Client('wss://xrplcluster.com');
    await client.connect();
    
    client.on('ledgerClosed', ledger => {
        console.log('New ledger:', ledger.ledger_index);
    });
    
    await client.request({
        command: 'subscribe',
        streams: ['ledger']
    });
}

subscribe();
```
{% endtab %}

{% tab title="xrpl-py" %}
```python
from xrpl.clients import JsonRpcClient
from xrpl.models.requests import Subscribe

client = JsonRpcClient("https://xrp.getblock.io/mainnet/")
request = Subscribe(streams=["ledger"])
response = client.request(request)
print(response.result)
```
{% endtab %}
{% endtabs %}
