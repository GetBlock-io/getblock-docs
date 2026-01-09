---
description: >-
  Example code for the sui_getNormalizedMoveModule JSON-RPC method. Complete
  guide on how to use sui_getNormalizedMoveModule JSON-RPC in GetBlock Web3
  documentation.
---

# sui\_getNormalizedMoveModule - Sui

This method returns a structured representation of a Move module on the SUI network. This provides complete module metadata including all structs, functions, and their details.

## Parameters

| Parameter    | Type     | Required | Description       |
| ------------ | -------- | -------- | ----------------- |
| package      | ObjectID | Yes      | Package object ID |
| module\_name | string   | Yes      | Module name       |

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
  "method": "sui_getNormalizedMoveModule",
  "params": [
    "0x0047d5fa0a823e7d0ff4d55c32b09995a0ae1eedfee9c7b1354e805ed10ee3d0",
    "module"
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
  method: 'sui_getNormalizedMoveModule',
  params: [
    '0x0047d5fa0a823e7d0ff4d55c32b09995a0ae1eedfee9c7b1354e805ed10ee3d0',
    'module'
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
    "method": "sui_getNormalizedMoveModule",
    "params": [
        "0x0047d5fa0a823e7d0ff4d55c32b09995a0ae1eedfee9c7b1354e805ed10ee3d0",
        "module"
    ]
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
        "method": "sui_getNormalizedMoveModule",
        "params": ["0x0047d5fa0a823e7d0ff4d55c32b09995a0ae1eedfee9c7b1354e805ed10ee3d0", "module"]
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
    "fileFormatVersion": 6,
    "address": "0x0047d5fa0a823e7d0ff4d55c32b09995a0ae1eedfee9c7b1354e805ed10ee3d0",
    "name": "module",
    "friends": [],
    "structs": {},
    "exposedFunctions": {}
  },
  "id": "getblock.io"
}
```
{% endcode %}

## Response Parameters

| Parameter         | Type    | Description                  |
| ----------------- | ------- | ---------------------------- |
| fileFormatVersion | integer | Move bytecode format version |
| address           | string  | Package address              |
| name              | string  | Module name                  |
| friends           | array   | Friend modules               |
| structs           | object  | Map of struct definitions    |
| exposedFunctions  | object  | Map of public functions      |

## Use Cases

* Generate module documentation
* Analyze contract structure
* Build contract explorers
* Validate module interfaces

## Error Handling

| Error Code     | Description      |
| -------------- | ---------------- |
| -32602         | Invalid params   |
| -32603         | Internal error   |
| ModuleNotFound | Module not found |

## SDK Integration

{% tabs %}
{% tab title="Sui TypeScript SDK" %}
{% code title="sdk.ts" %}
```typescript
import { SuiClient } from '@mysten/sui/client';

const client = new SuiClient({ url: 'https://go.getblock.io/<ACCESS-TOKEN>/' });
const module = await client.getNormalizedMoveModule({
  package: '0x0047d5fa0a823e7d0ff4d55c32b09995a0ae1eedfee9c7b1354e805ed10ee3d0',
  module: 'module'
});
console.log(module);
```
{% endcode %}
{% endtab %}

{% tab title="Sui Rust SDK" %}
{% code title="sdk.rs" overflow="wrap" %}
```rust
use sui_sdk::SuiClientBuilder;

#[tokio::main]
async fn main() -> Result<(), anyhow::Error> {
    let sui = SuiClientBuilder::default().build("https://go.getblock.io/<ACCESS-TOKEN>/").await?;
    let module = sui.read_api().get_normalized_move_module(
        "0x0047d5fa0a823e7d0ff4d55c32b09995a0ae1eedfee9c7b1354e805ed10ee3d0".parse()?,
        "module"
    ).await?;
    println!("{:?}", module);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
