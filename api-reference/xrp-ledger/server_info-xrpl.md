---
description: >-
  Example code for the server_info JSON RPC method. Complete guide to using
  server_info JSON-RPC in the GetBlock Web3 documentation.
---

# server\_info - XRPL

This method asks the server for a human-readable version of various information about the rippled server being queried.

## Parameters

* None

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
    "method": "server_info",
    "params": [{}],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="JavaScript (Axios)" %}
```javascript
const axios = require('axios');

const url = 'https://xrp.getblock.io/mainnet/';
const headers = {
    'x-api-key': 'YOUR-API-KEY',
    'Content-Type': 'application/json'
};

const payload = {
    jsonrpc: '2.0',
    method: 'server_info',
    params: [{}],
    id: 'getblock.io'
};

axios.post(url, payload, { headers })
    .then(response => console.log(response.data))
    .catch(error => console.error(error));
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="Python" %}
```python
import requests

url = "https://xrp.getblock.io/mainnet/"
headers = {
    "x-api-key": "YOUR-API-KEY",
    "Content-Type": "application/json"
}

payload = {
    "jsonrpc": "2.0",
    "method": "server_info",
    "params": [{}],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="Rust" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let payload = json!({
        "jsonrpc": "2.0",
        "method": "server_info",
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

{% code title="Response (JSON)" %}
```json
{
    "result": {
        "info": {
            "build_version": "1.9.4",
            "complete_ledgers": "32570-63632030",
            "load_factor": 1,
            "peers": 21,
            "server_state": "full",
            "time": "2024-Jan-15 12:00:00.000000 UTC",
            "uptime": 1234567,
            "validated_ledger": {
                "base_fee_xrp": 0.00001,
                "hash": "3A0B41C31B13436610C5961E2C914529CAD3CCC4A7859ED6AABFCD0700847BC1",
                "reserve_base_xrp": 10,
                "reserve_inc_xrp": 2,
                "seq": 63632030
            }
        },
        "status": "success"
    }
}
```
{% endcode %}

## Response Parameters

| Field             | Type   | Description            |
| ----------------- | ------ | ---------------------- |
| info              | object | Server information     |
| build\_version    | string | Server version         |
| complete\_ledgers | string | Available ledger range |
| load\_factor      | number | Load factor            |
| server\_state     | string | Server state           |
| validated\_ledger | object | Validated ledger info  |

## Use Cases

* Check server health
* Monitor node status
* Verify connectivity

## Error Handling

| Error Code | Description   |
| ---------- | ------------- |
| noNetwork  | Not connected |

## SDK Integration

{% tabs %}
{% tab title="xrpl.js" %}
{% code title="Node.js (xrpl.js)" %}
```javascript
const xrpl = require('xrpl');

async function getServerInfo() {
    const client = new xrpl.Client('wss://xrplcluster.com');
    await client.connect();
    
    const response = await client.request({
        command: 'server_info'
    });
    
    console.log('Server state:', response.result.info.server_state);
    await client.disconnect();
}

getServerInfo();
```
{% endcode %}
{% endtab %}

{% tab title="xrpl-py" %}
{% code title="Python (xrpl-py)" %}
```python
from xrpl.clients import JsonRpcClient
from xrpl.models.requests import ServerInfo

client = JsonRpcClient("https://xrp.getblock.io/mainnet/")
request = ServerInfo()
response = client.request(request)
print(response.result)
```
{% endcode %}
{% endtab %}
{% endtabs %}
