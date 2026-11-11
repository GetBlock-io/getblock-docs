---
description: >-
  Example code for the ping JSON RPC method. Complete guide to using ping
  JSON-RPC in the GetBlock Web3 documentation.
---

# ping - XRPL

This method returns an acknowledgement, so that clients can test the connection status and latency.

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="curl" %}
```bash
curl --location --request POST 'https://xrp.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "ping",
    "params": [{}],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="ping.js" %}
```javascript
const axios = require('axios');

const url = 'https://xrp.getblock.io/mainnet/';
const headers = {
    'x-api-key': 'YOUR-API-KEY',
    'Content-Type': 'application/json'
};

const payload = {
    jsonrpc: '2.0',
    method: 'ping',
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
{% code title="ping.py" %}
```python
import requests

url = "https://xrp.getblock.io/mainnet/"
headers = {
    "x-api-key": "YOUR-API-KEY",
    "Content-Type": "application/json"
}

payload = {
    "jsonrpc": "2.0",
    "method": "ping",
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
        "method": "ping",
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
        "status": "success"
    }
}
```

## Response Parameters

| Parameter | Type   | Description       |
| --------- | ------ | ----------------- |
| status    | string | Connection status |

## Use Cases

* Test connectivity
* Measure latency
* Health checks

## Error Handling

| Error Code | Description   |
| ---------- | ------------- |
| noNetwork  | Not connected |

## SDK Integration

{% tabs %}
{% tab title="xrpl.js" %}
{% code title="xrpl.js example" %}
```javascript
const xrpl = require('xrpl');

async function ping() {
    const client = new xrpl.Client('wss://xrplcluster.com');
    await client.connect();
    
    const response = await client.request({
        command: 'ping'
    });
    
    console.log('Ping:', response.result.status);
    await client.disconnect();
}

ping();
```
{% endcode %}
{% endtab %}

{% tab title="xrpl-py" %}
{% code title="xrpl-py example" %}
```python
from xrpl.clients import JsonRpcClient
from xrpl.models.requests import Ping

client = JsonRpcClient("https://xrp.getblock.io/mainnet/")
request = Ping()
response = client.request(request)
print(response.result)
```
{% endcode %}
{% endtab %}
{% endtabs %}
