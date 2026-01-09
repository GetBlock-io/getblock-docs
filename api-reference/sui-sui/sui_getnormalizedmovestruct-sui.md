---
description: >-
  Example code for the sui_getNormalizedMoveStruct JSON-RPC method. Complete
  guide on how to use sui_getNormalizedMoveStruct JSON-RPC in GetBlock Web3
  documentation.
---

# sui\_getNormalizedMoveStruct - Sui

The `sui_getNormalizedMoveStruct` method returns a structured representation of a Move struct on the SUI network. This provides complete struct metadata including abilities, type parameters, and field definitions.

## Parameters

| Parameter    | Type     | Required | Description       |
| ------------ | -------- | -------- | ----------------- |
| package      | ObjectID | Yes      | Package object ID |
| module\_name | string   | Yes      | Module name       |
| struct\_name | string   | Yes      | Struct name       |

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
  "method": "sui_getNormalizedMoveStruct",
  "params": [
    "0xc95b9e341bc3aba1654bdbad707dcd773bd6309363447ef3fe58a960de92aa93",
    "module",
    "StructName"
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
  method: 'sui_getNormalizedMoveStruct',
  params: [
    '0xc95b9e341bc3aba1654bdbad707dcd773bd6309363447ef3fe58a960de92aa93',
    'module',
    'StructName'
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
    "method": "sui_getNormalizedMoveStruct",
    "params": [
        "0xc95b9e341bc3aba1654bdbad707dcd773bd6309363447ef3fe58a960de92aa93",
        "module",
        "StructName"
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
        "method": "sui_getNormalizedMoveStruct",
        "params": ["0xc95b9e341bc3aba1654bdbad707dcd773bd6309363447ef3fe58a960de92aa93", "module", "StructName"]
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
    "abilities": {
      "abilities": ["copy", "drop", "store", "key"]
    },
    "typeParameters": [],
    "fields": [
      {
        "name": "id",
        "type": { "Struct": { "address": "0x2", "module": "object", "name": "UID" } }
      },
      {
        "name": "balance",
        "type": "U64"
      }
    ]
  },
  "id": "getblock.io"
}
```

## Response Parameters

| Parameter      | Type   | Description                               |
| -------------- | ------ | ----------------------------------------- |
| abilities      | object | Struct abilities (copy, drop, store, key) |
| typeParameters | array  | Generic type parameters                   |
| fields         | array  | Array of field definitions                |

## Use Cases

* Understand object structure
* Generate type definitions
* Build deserialization logic
* Document data types

## Error Handling

| Error Code     | Description      |
| -------------- | ---------------- |
| -32602         | Invalid params   |
| -32603         | Internal error   |
| StructNotFound | Struct not found |

## SDK Integration

{% tabs %}
{% tab title="Sui TypeScript SDK" %}
{% code title="typescript.ts" %}
```typescript
import { SuiClient } from '@mysten/sui/client';

const client = new SuiClient({ url: 'https://go.getblock.io/<ACCESS-TOKEN>/' });
const struct = await client.getNormalizedMoveStruct({
  package: '0xc95b9e341bc3aba1654bdbad707dcd773bd6309363447ef3fe58a960de92aa93',
  module: 'module',
  struct: 'StructName'
});
console.log(struct);
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
    let s = sui.read_api().get_normalized_move_struct(
        "0xc95b9e341bc3aba1654bdbad707dcd773bd6309363447ef3fe58a960de92aa93".parse()?,
        "module",
        "StructName"
    ).await?;
    println!("{:?}", s);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
