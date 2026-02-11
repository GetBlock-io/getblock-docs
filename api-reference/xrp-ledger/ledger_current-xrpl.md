---
description: >-
  Example code for the ledger_current JSON RPC method. Сomplete guide on how to
  use ledger_current JSON RPC in GetBlock Web3 documentation.
---

# ledger\_current - XRPL

This method returns the unique identifiers of the current in-progress ledger. This command is mostly useful for testing, because the ledger returned is still in flux.

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
    "method": "ledger_current",
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
    method: 'ledger_current',
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
    "method": "ledger_current",
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
        "method": "ledger_current",
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
        "ledger_current_index": 102024660,
        "status": "success"
    }
}
```

## Response Parameters

| Parameter              | Type   | Description             |
| ---------------------- | ------ | ----------------------- |
| ledger\_current\_index | number | Current ledger sequence |

## Use Cases

* Get current ledger
* Testing purposes
* Monitor network state

## Error Handling

| Error Code | Description   |
| ---------- | ------------- |
| noNetwork  | Not connected |

## SDK Integration

{% tabs %}
{% tab title="xrpl.js" %}
```javascript
const xrpl = require('xrpl');

async function getCurrentLedger() {
    const client = new xrpl.Client('wss://xrplcluster.com');
    await client.connect();
    
    const response = await client.request({
        command: 'ledger_current'
    });
    
    console.log('Current ledger:', response.result.ledger_current_index);
    await client.disconnect();
}

getCurrentLedger();
```
{% endtab %}

{% tab title="xrpl-py" %}
```python
from xrpl.clients import JsonRpcClient
from xrpl.models.requests import LedgerCurrent

client = JsonRpcClient("https://xrp.getblock.io/mainnet/")
request = LedgerCurrent()
response = client.request(request)
print(response.result)
```
{% endtab %}
{% endtabs %}
