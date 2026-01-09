---
description: >-
  Example code for the sui_getMoveFunctionArgTypes JSON-RPC method. Complete
  guide on how to use sui_getMoveFunctionArgTypes JSON-RPC in GetBlock Web3
  documentation.
---

# sui\_getMoveFunctionArgTypes - Sui

This method returns the argument types of a Move function on the SUI network. This is useful for understanding function signatures, building transaction calls, and validating inputs before execution.

## Parameters

| Parameter | Type     | Required | Description       |
| --------- | -------- | -------- | ----------------- |
| package   | ObjectID | Yes      | Package object ID |
| module    | string   | Yes      | Module name       |
| function  | string   | Yes      | Function name     |

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
  "method": "sui_getMoveFunctionArgTypes",
  "params": [
    "0xa0a7b108f5023b7356f2c6a4be6f058e267aae38e08260c7d519d8641897490c",
    "suifrens",
    "mint"
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
  method: 'sui_getMoveFunctionArgTypes',
  params: [
    '0xa0a7b108f5023b7356f2c6a4be6f058e267aae38e08260c7d519d8641897490c',
    'suifrens',
    'mint'
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
    "method": "sui_getMoveFunctionArgTypes",
    "params": [
        "0xa0a7b108f5023b7356f2c6a4be6f058e267aae38e08260c7d519d8641897490c",
        "suifrens",
        "mint"
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
        "method": "sui_getMoveFunctionArgTypes",
        "params": [
            "0xa0a7b108f5023b7356f2c6a4be6f058e267aae38e08260c7d519d8641897490c",
            "suifrens",
            "mint"
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

{% code title="response.json" %}
```json
{
  "jsonrpc": "2.0",
  "result": [
    "Pure",
    { "Object": "ByMutableReference" },
    { "Object": "ByValue" }
  ],
  "id": "getblock.io"
}
```
{% endcode %}

## Response Parameters

| Parameter | Type  | Description                        |
| --------- | ----- | ---------------------------------- |
| result    | array | Array of argument type descriptors |

### Argument Types

* `Pure` - Primitive type (u8, u64, bool, address, vector, etc.)
* `Object: ByValue` - Object passed by value (consumed)
* `Object: ByImmutableReference` - Immutable object reference
* `Object: ByMutableReference` - Mutable object reference

## Use Cases

* Build transaction calls programmatically
* Validate function parameters
* Generate type-safe SDK code
* Document smart contract interfaces

## Error Handling

| Error Code       | Description                                      |
| ---------------- | ------------------------------------------------ |
| -32602           | Invalid params - invalid package/module/function |
| -32603           | Internal error - node issues                     |
| FunctionNotFound | Function does not exist                          |

## SDK Integration

{% tabs %}
{% tab title="Sui TypeScript SDK" %}
{% code title="sdk.ts" %}
```typescript
import { SuiClient } from '@mysten/sui/client';

const client = new SuiClient({ url: 'https://go.getblock.io/<ACCESS-TOKEN>/' });
const argTypes = await client.getMoveFunctionArgTypes({
  package: '0xa0a7b108f5023b7356f2c6a4be6f058e267aae38e08260c7d519d8641897490c',
  module: 'suifrens',
  function: 'mint'
});
console.log(argTypes);
```
{% endcode %}
{% endtab %}

{% tab title="Sui Rust SDK" %}
{% code title="sdk.rs" %}
```rust
use sui_sdk::SuiClientBuilder;

#[tokio::main]
async fn main() -> Result<(), anyhow::Error> {
    let sui = SuiClientBuilder::default().build("https://go.getblock.io/<ACCESS-TOKEN>/").await?;
    let arg_types = sui.read_api().get_move_function_arg_types(
        "0xa0a7b108f5023b7356f2c6a4be6f058e267aae38e08260c7d519d8641897490c".parse()?,
        "suifrens",
        "mint"
    ).await?;
    println!("{:?}", arg_types);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
