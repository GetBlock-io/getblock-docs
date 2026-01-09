---
description: >-
  Example code for the suix_resolveNameServiceNames JSON-RPC method. Complete
  guide on how to use suix_resolveNameServiceNames JSON-RPC in GetBlock Web3
  documentation.
---

# suix\_resolveNameServiceNames - Sui

This method performs reverse resolution to find all SuiNS names associated with a given SUI address. This is the inverse of `suix_resolveNameServiceAddress` and is useful for displaying human-readable names in user interfaces and transaction histories.

## Parameters

| Parameter | Type       | Required | Description                |
| --------- | ---------- | -------- | -------------------------- |
| address   | SuiAddress | Yes      | The SUI address to resolve |
| cursor    | ObjectID   | No       | Pagination cursor          |
| limit     | uint       | No       | Maximum items per page     |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "suix_resolveNameServiceNames",
  "params": [
    "0x5cd6fa76ed1d18f05f15e35075252ddec4fb83621d55952d9172fcfcb72feae2",
    null,
    10
  ]
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
  method: 'suix_resolveNameServiceNames',
  params: ['0x5cd6fa76ed1d18f05f15e35075252ddec4fb83621d55952d9172fcfcb72feae2', null, 10]
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
    "method": "suix_resolveNameServiceNames",
    "params": [
        "0x5cd6fa76ed1d18f05f15e35075252ddec4fb83621d55952d9172fcfcb72feae2",
        None,
        10
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
        "method": "suix_resolveNameServiceNames",
        "params": ["0x5cd6fa76ed1d18f05f15e35075252ddec4fb83621d55952d9172fcfcb72feae2", null, 10]
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
    "data": ["alice.sui", "alice-main.sui"],
    "nextCursor": null,
    "hasNextPage": false
  },
  "id": "getblock.io"
}
```

## Response Parameters

| Parameter   | Type    | Description                 |
| ----------- | ------- | --------------------------- |
| data        | array   | Array of SuiNS name strings |
| nextCursor  | string  | Cursor for pagination       |
| hasNextPage | boolean | More names available        |

## Use Cases

* Display user-friendly names in UIs
* Show names in transaction histories
* Build profile pages with all owned names
* Verify name ownership

## Error Handling

| Error Code | Description                        |
| ---------- | ---------------------------------- |
| -32602     | Invalid params - malformed address |
| -32603     | Internal error - node issues       |

## SDK Integration

{% tabs %}
{% tab title="Sui TypeScript SDK" %}
{% code title="example.ts" %}
```typescript
import { SuiClient } from '@mysten/sui/client';

const client = new SuiClient({ url: 'https://go.getblock.io/<ACCESS-TOKEN>/' });
const names = await client.resolveNameServiceNames({
  address: '0x5cd6fa76ed1d18f05f15e35075252ddec4fb83621d55952d9172fcfcb72feae2'
});
console.log(names);
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
    let address = "0x5cd6fa76ed1d18f05f15e35075252ddec4fb83621d55952d9172fcfcb72feae2".parse()?;
    let names = sui.read_api().resolve_name_service_names(address, None, None).await?;
    println!("{:?}", names);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
