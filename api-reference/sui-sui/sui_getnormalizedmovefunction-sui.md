---
description: >-
  Example code for the sui_getNormalizedMoveFunction JSON-RPC method. Complete
  guide on how to use sui_getNormalizedMoveFunction JSON-RPC in GetBlock Web3
  documentation.
---

# sui\_getNormalizedMoveFunction - Sui

This method returns a structured representation of a Move function on the SUI network. This provides complete function metadata, including visibility, type parameters, parameter types, and return types.

## Parameters

| Parameter      | Type     | Required | Description       |
| -------------- | -------- | -------- | ----------------- |
| package        | ObjectID | Yes      | Package object ID |
| module\_name   | string   | Yes      | Module name       |
| function\_name | string   | Yes      | Function name     |

### Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "sui_getNormalizedMoveFunction",
  "params": [
    "0x9c4eb6769ca8b6a23efeb7298cf0a8d0b837b78749c2cfc711c42036cc6b7621",
    "moduleName",
    "functionName"
  ]
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code overflow="wrap" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "sui_getNormalizedMoveFunction",
  "params": [
    "0x9c4eb6769ca8b6a23efeb7298cf0a8d0b837b78749c2cfc711c42036cc6b7621",
    "moduleName",
    "functionName"
  ]
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers: {
        'Content-Type': 'application/json'
    },
    data: data
};

axios(config)
    .then(response => console.log(JSON.stringify(response.data)))
    .catch(error => console.log(error));
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code overflow="wrap" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "sui_getNormalizedMoveFunction",
  "params": [
    "0x9c4eb6769ca8b6a23efeb7298cf0a8d0b837b78749c2cfc711c42036cc6b7621",
    "moduleName",
    "functionName"
  ]
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code overflow="wrap" %}
```rust
use reqwest::header;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = reqwest::Client::new();
    
    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header(header::CONTENT_TYPE, "application/json")
        .body(r#"{
          "jsonrpc": "2.0",
          "id": "getblock.io",
          "method": "sui_getNormalizedMoveFunction",
          "params": [
            "0x9c4eb6769ca8b6a23efeb7298cf0a8d0b837b78749c2cfc711c42036cc6b7621",
            "moduleName",
            "functionName"
          ]
        }"#)
        .send()
        .await?;
    
    println!("{}", response.text().await?);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Response Example

{% code title="Response (JSON)" overflow="wrap" %}
```json
{
  "jsonrpc": "2.0",
  "result": {
    "visibility": "Public",
    "isEntry": true,
    "typeParameters": [
      { "abilities": ["copy", "drop", "store"] }
    ],
    "parameters": [
      { "MutableReference": { "Struct": { "address": "0x2", "module": "coin", "name": "Coin" } } },
      "U64"
    ],
    "return": []
  },
  "id": "getblock.io"
}
```
{% endcode %}

### Response Parameters

| Parameter      | Type    | Description                        |
| -------------- | ------- | ---------------------------------- |
| visibility     | string  | Public, Private, or Friend         |
| isEntry        | boolean | Whether callable as entry point    |
| typeParameters | array   | Generic type parameter constraints |
| parameters     | array   | Function parameter types           |
| return         | array   | Return types                       |

## Use Cases

* Inspect function signatures
* Generate API documentation
* Build type-safe transaction builders
* Analyze smart contract interfaces

## Error Handling

| Error Code       | Description        |
| ---------------- | ------------------ |
| -32602           | Invalid params     |
| -32603           | Internal error     |
| FunctionNotFound | Function not found |

## SDK Integration

{% tabs %}
{% tab title="Sui TypeScript SDK" %}
{% code title="TypeScript (SUI SDK)" %}
```typescript
import { SuiClient } from '@mysten/sui/client';

const client = new SuiClient({ url: 'https://go.getblock.io/<ACCESS-TOKEN>/' });
const func = await client.getNormalizedMoveFunction({
  package: '0x9c4eb6769ca8b6a23efeb7298cf0a8d0b837b78749c2cfc711c42036cc6b7621',
  module: 'moduleName',
  function: 'functionName'
});
console.log(func);
```
{% endcode %}
{% endtab %}

{% tab title="Sui Rust SDK" %}
{% code title="Rust (SUI SDK)" overflow="wrap" %}
```rust
use sui_sdk::SuiClientBuilder;

#[tokio::main]
async fn main() -> Result<(), anyhow::Error> {
    let sui = SuiClientBuilder::default().build("https://go.getblock.io/<ACCESS-TOKEN>/").await?;
    let func = sui.read_api().get_normalized_move_function(
        "0x9c4eb6769ca8b6a23efeb7298cf0a8d0b837b78749c2cfc711c42036cc6b7621".parse()?,
        "moduleName",
        "functionName"
    ).await?;
    println!("{:?}", func);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
