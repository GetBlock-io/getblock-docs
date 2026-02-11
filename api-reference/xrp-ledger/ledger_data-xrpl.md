---
description: >-
  Example code for the ledger_data JSON RPC method. Complete guide to using
  ledger_data JSON-RPC in the GetBlock Web3 documentation.
---

# ledger\_data - XRPL

This method retrieves contents of the specified ledger. You can iterate through several calls to retrieve the entire contents of a single ledger version.

## Parameters

| Parameter     | Type          | Required | Description              |
| ------------- | ------------- | -------- | ------------------------ |
| ledger\_hash  | string        | No       | Ledger hash              |
| ledger\_index | string/number | No       | Ledger index or shortcut |
| binary        | boolean       | No       | Return binary format     |
| limit         | number        | No       | Maximum results (10-400) |
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
    "method": "ledger_data",
    "params": [{
        "ledger_index": "validated",
        "limit": 10
    }],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
{% code title="ledger_data.js" %}
```javascript
const axios = require('axios');

const url = 'https://xrp.getblock.io/mainnet/';
const headers = {
    'x-api-key': 'YOUR-API-KEY',
    'Content-Type': 'application/json'
};

const payload = {
    jsonrpc: '2.0',
    method: 'ledger_data',
    params: [{
        ledger_index: 'validated',
        limit: 10
    }],
    id: 'getblock.io'
};

axios.post(url, payload, { headers })
    .then(response => console.log(response.data))
    .catch(error => console.error(error));
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="ledger_data.py" %}
```python
import requests

url = "https://xrp.getblock.io/mainnet/"
headers = {
    "x-api-key": "YOUR-API-KEY",
    "Content-Type": "application/json"
}

payload = {
    "jsonrpc": "2.0",
    "method": "ledger_data",
    "params": [{
        "ledger_index": "validated",
        "limit": 10
    }],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="ledger_data.rs" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let payload = json!({
        "jsonrpc": "2.0",
        "method": "ledger_data",
        "params": [{
            "ledger_index": "validated",
            "limit": 10
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
        "ledger": {
            "closed": false,
            "ledger_index": "63632030"
        },
        "ledger_current_index": 63632030,
        "ledger_hash": "3A0B41C31B13436610C5961E2C914529CAD3CCC4A7859ED6AABFCD0700847BC1",
        "ledger_index": 63632030,
        "marker": "0003EDC64561B6DBB6CF257EC0F7C0A0A08B88DEAD1D59C14794256E3BACAEA5",
        "state": [
            {
                "Account": "rMj5DFATVxw91PDy3AM2wu7Uu1kgrhWypE",
                "Balance": "20722989",
                "LedgerEntryType": "AccountRoot"
            }
        ],
        "status": "success"
    }
}
```

## Response Parameters

| Parameter | Type   | Description         |
| --------- | ------ | ------------------- |
| state     | array  | Ledger entries      |
| marker    | object | Continue pagination |

| Field         | Type   | Description       |
| ------------- | ------ | ----------------- |
| ledger\_index | number | Ledger index      |
| ledger\_hash  | string | Ledger hash       |
| state         | array  | Ledger entries    |
| marker        | object | Pagination marker |

## Use Cases

* Iterate ledger contents
* Full ledger analysis
* State snapshots

## Error Handling

| Error Code    | Description        |
| ------------- | ------------------ |
| lgrNotFound   | Ledger not found   |
| invalidParams | Invalid parameters |

## SDK Integration

{% tabs %}
{% tab title="xrpl.js" %}
{% code title="xrpl.js example" %}
```javascript
const xrpl = require('xrpl');

async function getLedgerData() {
    const client = new xrpl.Client('wss://xrplcluster.com');
    await client.connect();
    
    const response = await client.request({
        command: 'ledger_data',
        ledger_index: 'validated',
        limit: 10
    });
    
    console.log('Entries:', response.result.state.length);
    await client.disconnect();
}

getLedgerData();
```
{% endcode %}
{% endtab %}

{% tab title="xrpl.py" %}
{% code title="xrpl-py example" %}
```python
from xrpl.clients import JsonRpcClient
from xrpl.models.requests import LedgerData

client = JsonRpcClient("https://xrp.getblock.io/mainnet/")
request = LedgerData(ledger_index="validated", limit=10)
response = client.request(request)
print(response.result)
```
{% endcode %}
{% endtab %}
{% endtabs %}
