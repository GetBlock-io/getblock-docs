---
description: >-
  Example code for the manifest JSON RPC method. Сomplete guide on how to use
  manifest JSON RPC in GetBlock Web3 documentation.
---

# manifest - XRPL

This method retrieves the latest ephemeral public key information about a known validator.

## Parameters

| Parameter   | Type   | Required | Description          |
| ----------- | ------ | -------- | -------------------- |
| public\_key | string | Yes      | Validator public key |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://xrp.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "manifest",
    "params": [{
        "public_key": "nHUFE9prPXPrHcG3SkwP1UzAQbSphqyQkQK9ATXLZsfkezhhda3p"
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
    method: 'manifest',
    params: [{
        public_key: 'nHUFE9prPXPrHcG3SkwP1UzAQbSphqyQkQK9ATXLZsfkezhhda3p'
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
    "method": "manifest",
    "params": [{
        "public_key": "nHUFE9prPXPrHcG3SkwP1UzAQbSphqyQkQK9ATXLZsfkezhhda3p"
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
        "method": "manifest",
        "params": [{
            "public_key": "nHUFE9prPXPrHcG3SkwP1UzAQbSphqyQkQK9ATXLZsfkezhhda3p"
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
        "details": {
            "domain": "alloy.ee",
            "ephemeral_key": "n9LMfcjE6dMyshCqiftLFXpB9K3Mnd2r5bG7K8osmrkFpHUoR3c1",
            "master_key": "nHUFE9prPXPrHcG3SkwP1UzAQbSphqyQkQK9ATXLZsfkezhhda3p",
            "seq": 4
        },
        "manifest": "JAAAAARxIe3AkJgOyqs3y+UuiAI27Ff3Mrfbt8e7mjdo06bnGEp5XnMhAu2LSAwZEQnm/mq9K6sSZJk5JkbcKCv6C7vQW2C8RnZVdkcwRQIhAI9uwQ1p58oyob1E+DaFLwjTdiRbVIKSMPqaaUwnJdN2AiB79DlPXHwztNULraVTkehbDsCAyDdf3VZB3FvkCZNOFHcIYWxsb3kuZWVwEkBf6A9ktcj2H4a61Av8ujQFL2KNcmr/FuEKbwlZEniJvhf0UqNiYc2bAsTJE5wMn00E0JBbw2m9OFwto50DcdkC",
        "requested": "nHUFE9prPXPrHcG3SkwP1UzAQbSphqyQkQK9ATXLZsfkezhhda3p",
        "status": "success"
    }
}
```

## Returns

| Field     | Type   | Description             |
| --------- | ------ | ----------------------- |
| details   | object | Manifest details        |
| manifest  | string | Base64-encoded manifest |
| requested | string | Requested public key    |

## Use Cases

* Validator verification
* Network monitoring
* UNL management

## SDK Integration

{% tabs %}
{% tab title="xrpl.js" %}
```javascript
const xrpl = require('xrpl');

async function getManifest() {
    const client = new xrpl.Client('wss://xrplcluster.com');
    await client.connect();
    
    const response = await client.request({
        command: 'manifest',
        public_key: 'nHUFE9prPXPrHcG3SkwP1UzAQbSphqyQkQK9ATXLZsfkezhhda3p'
    });
    
    console.log('Manifest:', response.result);
    await client.disconnect();
}

getManifest();
```
{% endtab %}
{% endtabs %}
