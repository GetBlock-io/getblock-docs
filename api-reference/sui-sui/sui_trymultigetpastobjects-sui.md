---
description: >-
  Example code for the sui_tryMultiGetPastObjects JSON-RPC method. Complete
  guide on how to use sui_tryMultiGetPastObjects JSON-RPC in GetBlock Web3
  documentation.
---

# sui\_tryMultiGetPastObjects - Sui

This method returns object data for multiple objects at their specified versions on the SUI network. This batch method combines the functionality of `sui_tryGetPastObject` for multiple objects in a single request, making it efficient for historical state queries.

## Parameters

| Parameter     | Type              | Required | Description                        |
| ------------- | ----------------- | -------- | ---------------------------------- |
| past\_objects | array             | Yes      | Array of {objectId, version} pairs |
| options       | ObjectDataOptions | No       | Options for response content       |

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
  "method": "sui_tryMultiGetPastObjects",
  "params": [
    [
      {
        "objectId": "0x53e4567ccafa5f36ce84c80aa8bc9be64e0d5ae796884274aef3005ae6733809",
        "version": "4"
      }
    ],
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
  method: 'sui_tryMultiGetPastObjects',
  params: [
    [{ objectId: '0x53e4567ccafa5f36ce84c80aa8bc9be64e0d5ae796884274aef3005ae6733809', version: '4' }],
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
    "method": "sui_tryMultiGetPastObjects",
    "params": [
        [{"objectId": "0x53e4567ccafa5f36ce84c80aa8bc9be64e0d5ae796884274aef3005ae6733809", "version": "4"}],
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
        "method": "sui_tryMultiGetPastObjects",
        "params": [
            [{"objectId": "0x53e4567...", "version": "4"}],
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
  "result": [
    {
      "status": "VersionFound",
      "details": {
        "objectId": "0x53e4567ccafa5f36ce84c80aa8bc9be64e0d5ae796884274aef3005ae6733809",
        "version": "4",
        "content": { "dataType": "moveObject" }
      }
    }
  ],
  "id": "getblock.io"
}
```

## Response Parameters

| Parameter | Type   | Description                         |
| --------- | ------ | ----------------------------------- |
| status    | string | VersionFound, ObjectNotExists, etc. |
| details   | object | Object data at specified version    |

## Use Cases

* Batch historical state queries
* Audit multiple objects at once
* Compare object states across versions
* Historical portfolio snapshots

## Error Handling

| Error Code | Description                      |
| ---------- | -------------------------------- |
| -32602     | Invalid params - malformed input |
| -32603     | Internal error - node issues     |

## SDK Integration

{% tabs %}
{% tab title="Sui TypeScript SDK" %}
{% code title="example.ts" %}
```typescript
import { SuiClient } from '@mysten/sui/client';

const client = new SuiClient({ url: 'https://go.getblock.io/<ACCESS-TOKEN>/' });
const pastObjects = await client.tryMultiGetPastObjects({
  pastObjects: [{ objectId: '0x53e4567...', version: '4' }],
  options: { showContent: true }
});
console.log(pastObjects);
```
{% endcode %}
{% endtab %}

{% tab title="Sui Rust SDK" %}
{% code title="example.rs" overflow="wrap" %}
```rust
use sui_sdk::SuiClientBuilder;

#[tokio::main]
async fn main() -> Result<(), anyhow::Error> {
    let sui = SuiClientBuilder::default().build("https://go.getblock.io/<ACCESS-TOKEN>/").await?;
    // Use multi_try_get_parsed_past_object
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
