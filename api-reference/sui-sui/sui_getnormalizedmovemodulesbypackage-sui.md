---
description: >-
  Example code for the sui_getNormalizedMoveModulesByPackage JSON-RPC method.
  Complete guide on how to use sui_getNormalizedMoveModulesByPackage JSON-RPC in
  GetBlock Web3 documentation.
---

# sui\_getNormalizedMoveModulesByPackage - Sui

This method returns all modules in a Move package on the SUI network. This provides a complete view of a package's module structure and is useful for package analysis and documentation.

## Parameters

| Parameter | Type     | Required | Description       |
| --------- | -------- | -------- | ----------------- |
| package   | ObjectID | Yes      | Package object ID |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "sui_getNormalizedMoveModulesByPackage",
  "params": ["0x61630d3505f8905a0f4d42c6ff39a78a6ba2b28f68a3299ec3417bbabc6717dc"]
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
const axios = require('axios');

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'sui_getNormalizedMoveModulesByPackage',
  params: ['0x61630d3505f8905a0f4d42c6ff39a78a6ba2b28f68a3299ec3417bbabc6717dc']
};

axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', payload)
.then(response => console.log(response.data));
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "sui_getNormalizedMoveModulesByPackage",
    "params": ["0x61630d3505f8905a0f4d42c6ff39a78a6ba2b28f68a3299ec3417bbabc6717dc"]
}

response = requests.post("https://go.getblock.io/<ACCESS-TOKEN>/", json=payload)
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
        "id": "getblock.io",
        "method": "sui_getNormalizedMoveModulesByPackage",
        "params": ["0x61630d3505f8905a0f4d42c6ff39a78a6ba2b28f68a3299ec3417bbabc6717dc"]
    });
    let response = client.post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .json(&payload).send().await?;
    println!("{}", response.text().await?);
    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Response Example

```json
{
  "jsonrpc": "2.0",
  "result": {
    "module_a": {
      "fileFormatVersion": 6,
      "address": "0x61630d3505f8905a0f4d42c6ff39a78a6ba2b28f68a3299ec3417bbabc6717dc",
      "name": "module_a",
      "friends": [],
      "structs": {},
      "exposedFunctions": {}
    },
    "module_b": {
      "fileFormatVersion": 6,
      "address": "0x61630d3505f8905a0f4d42c6ff39a78a6ba2b28f68a3299ec3417bbabc6717dc",
      "name": "module_b",
      "friends": [],
      "structs": {},
      "exposedFunctions": {}
    }
  },
  "id": "getblock.io"
}
```

## Response Parameters

| Parameter | Type   | Description                               |
| --------- | ------ | ----------------------------------------- |
| result    | object | Map of module names to module definitions |

## Use Cases

* List all modules in a package
* Generate complete package documentation
* Analyze package architecture
* Build package explorers

## Error Handling

| Error Code      | Description       |
| --------------- | ----------------- |
| -32602          | Invalid params    |
| -32603          | Internal error    |
| PackageNotFound | Package not found |

## SDK Integration

{% tabs %}
{% tab title="Sui TypeScript SDK" %}
```typescript
import { SuiClient } from '@mysten/sui/client';

const client = new SuiClient({ url: 'https://go.getblock.io/<ACCESS-TOKEN>/' });
const modules = await client.getNormalizedMoveModulesByPackage({
  package: '0x61630d3505f8905a0f4d42c6ff39a78a6ba2b28f68a3299ec3417bbabc6717dc'
});
console.log(modules);
```
{% endtab %}

{% tab title="Sui Rust SDK" %}
```rust
use sui_sdk::SuiClientBuilder;

#[tokio::main]
async fn main() -> Result<(), anyhow::Error> {
    let sui = SuiClientBuilder::default().build("https://go.getblock.io/<ACCESS-TOKEN>/").await?;
    let modules = sui.read_api().get_normalized_move_modules_by_package(
        "0x61630d3505f8905a0f4d42c6ff39a78a6ba2b28f68a3299ec3417bbabc6717dc".parse()?
    ).await?;
    println!("{:?}", modules);
    Ok(())
}
```
{% endtab %}
{% endtabs %}
