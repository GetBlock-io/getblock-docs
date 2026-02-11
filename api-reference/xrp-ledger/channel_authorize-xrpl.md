---
description: >-
  Example code for the channel_authorize JSON RPC method. Complete guide to
  using channel_authorize JSON-RPC in the GetBlock Web3 documentation.
---

# channel\_authorize - XRPL

This method creates a signature that can be used to redeem a specific amount of XRP from a payment channel.

## Parameters

| Parameter   | Type   | Required | Description                         |
| ----------- | ------ | -------- | ----------------------------------- |
| channel\_id | string | Yes      | Channel ID (64 hex)                 |
| amount      | string | Yes      | Amount to authorize in drops        |
| secret      | string | No       | Secret key (if not using key\_type) |
| seed        | string | No       | Seed value                          |
| seed\_hex   | string | No       | Seed in hex                         |
| passphrase  | string | No       | Passphrase                          |
| key\_type   | string | No       | Key algorithm                       |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://xrp.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "channel_authorize",
    "params": [{
        "channel_id": "5DB01B7FFED6B67E6B0414DED11E051D2EE2B7619CE0EAA6286D67A3A4D5BDB3",
        "amount": "1000000",
        "secret": "s..."
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
    method: 'channel_authorize',
    params: [{
        channel_id: '5DB01B7FFED6B67E6B0414DED11E051D2EE2B7619CE0EAA6286D67A3A4D5BDB3',
        amount: '1000000',
        secret: 's...'
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
    "method": "channel_authorize",
    "params": [{
        "channel_id": "5DB01B7FFED6B67E6B0414DED11E051D2EE2B7619CE0EAA6286D67A3A4D5BDB3",
        "amount": "1000000",
        "secret": "s..."
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
        "method": "channel_authorize",
        "params": [{
            "channel_id": "5DB01B7FFED6B67E6B0414DED11E051D2EE2B7619CE0EAA6286D67A3A4D5BDB3",
            "amount": "1000000",
            "secret": "s..."
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

{% code title="response.json" overflow="wrap" %}
```json
{
    "result": {
        "signature": "304402204EF0AFB78AC23ED1C472E74F4299C0C21F1B21D07EFC0A3838A420F76D783A400220154FB11B6F54320666E4C36CA7F686C16A3A0456800BBC43746F34AF50290064",
        "status": "success"
    }
}
```
{% endcode %}

## Returns

| Field     | Type   | Description             |
| --------- | ------ | ----------------------- |
| signature | string | Authorization signature |

## Use Cases

* Payment channel claims
* Micropayments
* Streaming payments

## SDK Integration

### xrpl.js

{% hint style="info" %}
Note: channel\_authorize requires admin or local signing. Use local signing (client-side) when possible.
{% endhint %}

```javascript
const xrpl = require('xrpl');

async function authorizeChannel() {
    const client = new xrpl.Client('wss://xrplcluster.com');
    await client.connect();
    
    // Note: channel_authorize requires admin or local signing
    // Use xrpl.authorizeChannel() for local signing
    const signature = xrpl.authorizeChannel(
        wallet,
        '5DB01B7FFED6B67E6B0414DED11E051D2EE2B7619CE0EAA6286D67A3A4D5BDB3',
        '1000000'
    );
    
    console.log('Signature:', signature);
    await client.disconnect();
}

authorizeChannel();
```
