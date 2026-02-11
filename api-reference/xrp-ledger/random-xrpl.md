---
description: >-
  Example code for the random JSON RPC method. Сomplete guide on how to use
  random JSON RPC in GetBlock Web3 documentation.
---

# random - XRPL

This method provides a random number to clients as a source of entropy for random number generation.

## Parameters

* None

## Returns

| Field  | Type   | Description               |
| ------ | ------ | ------------------------- |
| random | string | 256-bit random hex string |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://xrp.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "random",
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
    method: 'random',
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
    "method": "random",
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
        "method": "random",
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

{% code overflow="wrap" %}
```json
{
    "result": {
        "random": "0FE8873454224ABED497D6F3F84CD8A843DE0EFAC874114433FA83F59824363F",
        "status": "success",
        "warning": "load"
    }
}
```
{% endcode %}

## Response Parameters

| Parameter | Type   | Description       |
| --------- | ------ | ----------------- |
| random    | string | Random hex string |

## Use Cases

* Entropy source
* Random key generation
* Cryptographic operations

## Error Handling

| Error Code | Description    |
| ---------- | -------------- |
| internal   | Internal error |

## SDK Integration

{% tabs %}
{% tab title="xrpl.js" %}
```javascript
const xrpl = require('xrpl');

async function getRandom() {
    const client = new xrpl.Client('wss://xrplcluster.com');
    await client.connect();
    
    const response = await client.request({
        command: 'random'
    });
    
    console.log('Random:', response.result.random);
    await client.disconnect();
}

getRandom();
```
{% endtab %}

{% tab title="xrpl-py" %}
```python
from xrpl.clients import JsonRpcClient
from xrpl.models.requests import Random

client = JsonRpcClient("https://xrp.getblock.io/mainnet/")
request = Random()
response = client.request(request)
print(response.result)
```
{% endtab %}
{% endtabs %}
