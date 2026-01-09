---
description: >-
  Example code for the sui_getProtocolConfig JSON-RPC method. Complete guide on
  how to use sui_getProtocolConfig JSON-RPC in GetBlock Web3 documentation.
---

# sui\_getProtocolConfig - Sui

This method returns the protocol configuration table for a given version number on the SUI network. Protocol configurations define network parameters, limits, and feature flags that govern blockchain behavior. This is useful for understanding network capabilities and constraints.

## Parameters

| Parameter | Type                         | Required | Description                                  |
| --------- | ---------------------------- | -------- | -------------------------------------------- |
| version   | BigInt\_for\_uint64\<string> | No       | Protocol version number (defaults to latest) |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="curl" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "sui_getProtocolConfig",
  "params": [6]
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="request.js" %}
```javascript
const axios = require('axios');

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'sui_getProtocolConfig',
  params: [6]
};

axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', payload)
.then(response => console.log(response.data));
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="request.py" %}
```python
import requests

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "sui_getProtocolConfig",
    "params": [6]
}

response = requests.post("https://go.getblock.io/<ACCESS-TOKEN>/", json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="request.rs" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    let payload = json!({
        "jsonrpc": "2.0",
        "id": "getblock.io",
        "method": "sui_getProtocolConfig",
        "params": [6]
    });
    let response = client.post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .json(&payload).send().await?;
    println!("{}", response.text().await?);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code title="response.json" %}
```json
{
  "jsonrpc": "2.0",
  "result": {
    "minSupportedProtocolVersion": "1",
    "maxSupportedProtocolVersion": "104",
    "protocolVersion": "6",
    "featureFlags": {},
    "attributes": {}
  },
  "id": "getblock.io"
}
```
{% endcode %}

## Response Parameters

| Parameter                   | Type   | Description                                 |
| --------------------------- | ------ | ------------------------------------------- |
| minSupportedProtocolVersion | string | Minimum version this node supports          |
| maxSupportedProtocolVersion | string | Maximum version this node supports          |
| protocolVersion             | string | The queried version number                  |
| featureFlags                | object | Map of feature flag names to enabled status |
| attributes                  | object | Protocol configuration attributes           |

## Use Cases

* Check network capabilities and limits
* Validate protocol compatibility
* Monitor protocol upgrades
* Build version-aware applications

## Error Handling

| Error Code | Description                      |
| ---------- | -------------------------------- |
| -32602     | Invalid params - invalid version |
| -32603     | Internal error - node issues     |

## SDK Integration

{% tabs %}
{% tab title="Sui Typescript SDK" %}
{% code title="sui-sdk.ts" %}
```typescript
import { SuiClient } from '@mysten/sui/client';

const client = new SuiClient({ url: 'https://go.getblock.io/<ACCESS-TOKEN>/' });
const config = await client.getProtocolConfig({ version: 6 });
console.log(config);
```
{% endcode %}
{% endtab %}

{% tab title="Sui Rust SDK" %}
{% code title="sui-sdk.rs" overflow="wrap" %}
```rust
use sui_sdk::SuiClientBuilder;

#[tokio::main]
async fn main() -> Result<(), anyhow::Error> {
    let sui = SuiClientBuilder::default().build("https://go.getblock.io/<ACCESS-TOKEN>/").await?;
    let config = sui.read_api().get_protocol_config(Some(6)).await?;
    println!("{:?}", config);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
