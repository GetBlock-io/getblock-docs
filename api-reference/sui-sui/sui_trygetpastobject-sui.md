---
description: >-
  Example code for the sui_tryGetPastObject JSON-RPC method. Complete guide on
  how to use sui_tryGetPastObject JSON-RPC in GetBlock Web3 documentation.
---

# sui\_tryGetPastObject - Sui

This method returns the object data at a specific version on the SUI network. This is useful for inspecting historical state of objects, debugging, and auditing object changes over time. Unlike `sui_getObject` which returns current state, this method can access any previous version.

## Parameters

| Parameter  | Type              | Required | Description                  |
| ---------- | ----------------- | -------- | ---------------------------- |
| object\_id | ObjectID          | Yes      | The object ID to query       |
| version    | SequenceNumber    | Yes      | The version number to query  |
| options    | ObjectDataOptions | No       | Options for response content |

## Returns

| Field   | Type   | Description                                                      |
| ------- | ------ | ---------------------------------------------------------------- |
| status  | string | VersionFound, ObjectNotExists, ObjectDeleted, or VersionNotFound |
| details | object | Object data if found                                             |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "sui_tryGetPastObject",
  "params": [
    "0x53e4567ccafa5f36ce84c80aa8bc9be64e0d5ae796884274aef3005ae6733809",
    4,
    { "showContent": true }
  ]
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
  method: 'sui_tryGetPastObject',
  params: [
    '0x53e4567ccafa5f36ce84c80aa8bc9be64e0d5ae796884274aef3005ae6733809',
    4,
    { showContent: true }
  ]
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
    "method": "sui_tryGetPastObject",
    "params": [
        "0x53e4567ccafa5f36ce84c80aa8bc9be64e0d5ae796884274aef3005ae6733809",
        4,
        {"showContent": True}
    ]
}

response = requests.post("https://go.getblock.io/<ACCESS-TOKEN>/", json=payload)
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
        "id": "getblock.io",
        "method": "sui_tryGetPastObject",
        "params": [
            "0x53e4567ccafa5f36ce84c80aa8bc9be64e0d5ae796884274aef3005ae6733809",
            4,
            {"showContent": true}
        ]
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

```json
{
  "jsonrpc": "2.0",
  "result": {
    "status": "VersionFound",
    "details": {
      "objectId": "0x53e4567ccafa5f36ce84c80aa8bc9be64e0d5ae796884274aef3005ae6733809",
      "version": "4",
      "digest": "8WFzCvM4rP...",
      "content": {
        "dataType": "moveObject",
        "type": "0x2::coin::Coin<0x2::sui::SUI>",
        "fields": {
          "balance": "100000000"
        }
      }
    }
  },
  "id": "getblock.io"
}
```

## Response Parameters

| Parameter | Type   | Description                      |
| --------- | ------ | -------------------------------- |
| status    | string | Query result status              |
| details   | object | Object data at specified version |

## Use Cases

* Audit object state history
* Debug state changes
* Verify historical balances
* Track NFT ownership history

## Error Handling

| Error Code      | Description                              |
| --------------- | ---------------------------------------- |
| -32602          | Invalid params - malformed ID or version |
| -32603          | Internal error - node issues             |
| VersionNotFound | Requested version not available          |
| ObjectDeleted   | Object was deleted before version        |

## SDK Integration

{% tabs %}
{% tab title="Sui TypeScript SDK" %}
{% code title="sui.ts" %}
```typescript
import { SuiClient } from '@mysten/sui/client';

const client = new SuiClient({ url: 'https://go.getblock.io/<ACCESS-TOKEN>/' });
const pastObject = await client.tryGetPastObject({
  id: '0x53e4567ccafa5f36ce84c80aa8bc9be64e0d5ae796884274aef3005ae6733809',
  version: 4,
  options: { showContent: true }
});
console.log(pastObject);
```
{% endcode %}
{% endtab %}

{% tab title="Sui Rust SDK" %}
{% code title="main.rs" overflow="wrap" %}
```rust
use sui_sdk::SuiClientBuilder;

#[tokio::main]
async fn main() -> Result<(), anyhow::Error> {
    let sui = SuiClientBuilder::default().build("https://go.getblock.io/<ACCESS-TOKEN>/").await?;
    let obj = sui.read_api().try_get_parsed_past_object(
        "0x53e4567...".parse()?, 4, None
    ).await?;
    println!("{:?}", obj);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
