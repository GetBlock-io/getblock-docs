---
description: >-
  Example code for the ledger_closed JSON RPC method. Complete guide to using
  ledger_closed JSON-RPC in the GetBlock Web3 documentation.
---

# ledger\_closed - XRPL

This method returns the unique identifiers of the most recently closed ledger. Note that the ledger returned is not necessarily validated and immutable yet.

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://xrp.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "ledger_closed",
    "params": [{}],
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
    method: 'ledger_closed',
    params: [{}],
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
    "method": "ledger_closed",
    "params": [{}],
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
        "method": "ledger_closed",
        "params": [{}],
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
        "ledger_hash": "A58833C1AE54D01EFFFD453DDAA33B72DF99BAE2D0F508C762A8F5F2A9353B09",
        "ledger_index": 102053796,
        "status": "success"
    }
}
```

## Response Parameters

| Parameter     | Type   | Description       |
| ------------- | ------ | ----------------- |
| ledger\_hash  | string | Ledger identifier |
| ledger\_index | number | Ledger sequence   |

## Use Cases

* Get latest closed ledger
* Monitor network progress
* Sync applications

## Error Handling

| Error Code | Description   |
| ---------- | ------------- |
| noNetwork  | Not connected |

## SDK Integration

{% tabs %}
{% tab title="xrpl.js" %}
```javascript
const xrpl = require('xrpl');

async function getClosedLedger() {
    const client = new xrpl.Client('wss://xrplcluster.com');
    await client.connect();
    
    const response = await client.request({
        command: 'ledger_closed'
    });
    
    console.log('Closed ledger:', response.result.ledger_index);
    await client.disconnect();
}

getClosedLedger();
```
{% endtab %}

{% tab title="xrpl-py" %}
```python
from xrpl.clients import JsonRpcClient
from xrpl.models.requests import LedgerClosed

client = JsonRpcClient("https://xrp.getblock.io/mainnet/")
request = LedgerClosed()
response = client.request(request)
print(response.result)
```
{% endtab %}
{% endtabs %}
