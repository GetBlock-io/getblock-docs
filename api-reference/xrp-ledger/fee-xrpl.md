---
description: >-
  Example code for the fee JSON RPC method. Complete guide to using fee JSON-RPC
  in the GetBlock Web3 documentation.
---

# fee - XRPL

This method reports the current state of the open-ledger requirements for the transaction cost.

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
    "method": "fee",
    "params": [{}],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="fee.js" %}
```javascript
const axios = require('axios');

const url = 'https://xrp.getblock.io/mainnet/';
const headers = {
    'x-api-key': 'YOUR-API-KEY',
    'Content-Type': 'application/json'
};

const payload = {
    jsonrpc: '2.0',
    method: 'fee',
    params: [{}],
    id: 'getblock.io'
};

axios.post(url, payload, { headers })
    .then(response => console.log(response.data))
    .catch(error => console.error(error));
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="fee.py" %}
```python
import requests

url = "https://xrp.getblock.io/mainnet/"
headers = {
    "x-api-key": "YOUR-API-KEY",
    "Content-Type": "application/json"
}

payload = {
    "jsonrpc": "2.0",
    "method": "fee",
    "params": [{}],
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
        "method": "fee",
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
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

```json
{
    "result": {
        "current_ledger_size": "186",
        "current_queue_size": "0",
        "drops": {
            "base_fee": "10",
            "median_fee": "5000",
            "minimum_fee": "10",
            "open_ledger_fee": "10"
        },
        "expected_ledger_size": "446",
        "ledger_current_index": 102053456,
        "levels": {
            "median_level": "128000",
            "minimum_level": "256",
            "open_ledger_level": "256",
            "reference_level": "256"
        },
        "max_queue_size": "8920",
        "status": "success"
    }
}
```

## Response Parameters

| Field                  | Type   | Description              |
| ---------------------- | ------ | ------------------------ |
| current\_ledger\_size  | string | Current ledger tx count  |
| current\_queue\_size   | string | Queued transaction count |
| drops                  | object | Fee amounts in drops     |
| expected\_ledger\_size | string | Expected ledger size     |
| ledger\_current\_index | number | Current ledger           |
| max\_queue\_size       | string | Maximum queue size       |
| base\_fee              | string | Base fee in drops        |
| minimum\_fee           | string | Minimum fee              |
| median\_fee            | string | Median fee               |
| open\_ledger\_fee      | string | Open ledger fee          |

## Use Cases

* Estimate transaction fees
* Monitor network congestion
* Optimize transaction costs

## Error Handling

| Error Code | Description   |
| ---------- | ------------- |
| noNetwork  | Not connected |

## SDK Integration

{% tabs %}
{% tab title="xrpl.js" %}
{% code title="xrpl-fee.js" %}
```javascript
const xrpl = require('xrpl');

async function getFee() {
    const client = new xrpl.Client('wss://xrplcluster.com');
    await client.connect();
    
    const response = await client.request({
        command: 'fee'
    });
    
    console.log('Base fee:', response.result.drops.base_fee, 'drops');
    await client.disconnect();
}

getFee();
```
{% endcode %}
{% endtab %}

{% tab title="xrpl-py" %}
{% code title="xrpl_fee.py" %}
```python
from xrpl.clients import JsonRpcClient
from xrpl.models.requests import Fee

client = JsonRpcClient("https://xrp.getblock.io/mainnet/")
request = Fee()
response = client.request(request)
print(response.result)
```
{% endcode %}
{% endtab %}
{% endtabs %}
