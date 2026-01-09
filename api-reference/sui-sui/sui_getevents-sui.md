---
description: >-
  Example code for the sui_getEvents JSON-RPC method. Complete guide on how to
  use sui_getEvents JSON-RPC in GetBlock Web3 documentation.
---

# sui\_getEvents - Sui

This method returns transaction events for a specified transaction digest on the SUI network. Events are emitted during transaction execution by Move modules and contain important information about state changes, transfers, and other on-chain activities.

## Parameters

| Parameter           | Type              | Required | Description                     |
| ------------------- | ----------------- | -------- | ------------------------------- |
| transaction\_digest | TransactionDigest | Yes      | The transaction digest to query |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "sui_getEvents",
  "params": ["11a72GCQ5hGNpWGh2QhQkkusTEGS6EDqifJqxr7nSYX"]
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="request.js" %}
```javascript
const axios = require('axios');

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'sui_getEvents',
  params: ['11a72GCQ5hGNpWGh2QhQkkusTEGS6EDqifJqxr7nSYX']
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
    "method": "sui_getEvents",
    "params": ["11a72GCQ5hGNpWGh2QhQkkusTEGS6EDqifJqxr7nSYX"]
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
        "method": "sui_getEvents",
        "params": ["11a72GCQ5hGNpWGh2QhQkkusTEGS6EDqifJqxr7nSYX"]
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
  "result": [
    {
      "id": {
        "txDigest": "11a72GCQ5hGNpWGh2QhQkkusTEGS6EDqifJqxr7nSYX",
        "eventSeq": "0"
      },
      "packageId": "0xc54ab30a3d9adc07c1429c4d6bbecaf9457c9af77a91f631760853934d383634",
      "transactionModule": "test_module",
      "sender": "0xbcf7c32655009a61f1de0eae420a2e4ae1bb772ab2dd5d5a7dfa949c0ef06908",
      "type": "0x9::test::TestEvent",
      "parsedJson": {
        "test": "example value"
      }
    }
  ],
  "id": "getblock.io"
}
```
{% endcode %}

## Response Parameters

| Parameter         | Type   | Description                            |
| ----------------- | ------ | -------------------------------------- |
| id                | object | Event identifier (txDigest + eventSeq) |
| packageId         | string | Package that emitted the event         |
| transactionModule | string | Module that emitted the event          |
| sender            | string | Transaction sender                     |
| type              | string | Event type                             |
| parsedJson        | object | Parsed event data                      |

## Use Cases

* Track transaction outcomes via events
* Monitor smart contract activities
* Build transaction history views
* Debug transaction execution

## Error Handling

| Error Code | Description                       |
| ---------- | --------------------------------- |
| -32602     | Invalid params - malformed digest |
| -32603     | Internal error - node issues      |

## SDK Integration

{% tabs %}
{% tab title="Sui TypeScript SDK" %}
{% code title="sdk.ts" overflow="wrap" %}
```typescript
import { SuiClient } from '@mysten/sui/client';

const client = new SuiClient({ url: 'https://go.getblock.io/<ACCESS-TOKEN>/' });
const events = await client.getEvents({ transactionDigest: '11a72GCQ5hGNpWGh2QhQkkusTEGS6EDqifJqxr7nSYX' });
console.log(events);
```
{% endcode %}
{% endtab %}

{% tab title="Sui Rust SDK" %}
{% code title="sdk.rs" %}
```rust
use sui_sdk::SuiClientBuilder;
use sui_types::digests::TransactionDigest;

#[tokio::main]
async fn main() -> Result<(), anyhow::Error> {
    let sui = SuiClientBuilder::default().build("https://go.getblock.io/<ACCESS-TOKEN>/").await?;
    let digest: TransactionDigest = "11a72GCQ5hGNpWGh2QhQkkusTEGS6EDqifJqxr7nSYX".parse()?;
    let events = sui.read_api().get_events(digest).await?;
    println!("{:?}", events);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
