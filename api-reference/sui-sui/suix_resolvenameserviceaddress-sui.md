---
description: >-
  Example code for the suix_queryTransactionBlocks JSON-RPC method. Complete
  guide on how to use suix_queryTransactionBlocks JSON-RPC in GetBlock Web3
  documentation.
---

# suix\_resolveNameServiceAddress - Sui

This method resolves a SuiNS (Sui Name Service) name to its corresponding SUI address. SuiNS provides human-readable names like `example.sui` that map to SUI addresses, similar to ENS on Ethereum. This method enables wallet applications and dApps to accept user-friendly names instead of hex addresses.

## Parameters

| Parameter | Type   | Required | Description                                     |
| --------- | ------ | -------- | ----------------------------------------------- |
| name      | string | Yes      | The SuiNS name to resolve (e.g., "example.sui") |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "suix_resolveNameServiceAddress",
  "params": ["example.sui"]
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'suix_resolveNameServiceAddress',
  params: ['example.sui']
};

axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', payload)
.then(response => console.log(response.data));
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "suix_resolveNameServiceAddress",
    "params": ["example.sui"]
}

response = requests.post("https://go.getblock.io/<ACCESS-TOKEN>/", json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="example.rs" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    let payload = json!({
        "jsonrpc": "2.0",
        "id": "getblock.io",
        "method": "suix_resolveNameServiceAddress",
        "params": ["example.sui"]
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
  "result": "0x6710024f81dd33ab6833482ee8034e779a48e6ef635c7f856df4905022458bfb",
  "id": "getblock.io"
}
```

## Response Parameters

| Parameter | Type   | Description                  |
| --------- | ------ | ---------------------------- |
| result    | string | Resolved SUI address or null |

## Use Cases

* Enable human-readable addresses in wallets
* Validate SuiNS name ownership
* Simplify payment interfaces
* Build address book features

## Error Handling

| Error Code  | Description                     |
| ----------- | ------------------------------- |
| -32602      | Invalid params - malformed name |
| -32603      | Internal error - node issues    |
| null result | Name not registered or expired  |

## SDK Integration

{% tabs %}
{% tab title="Sui TypeScript SDK" %}
{% code title="example.ts" %}
```typescript
import { SuiClient } from '@mysten/sui/client';

const client = new SuiClient({ url: 'https://go.getblock.io/<ACCESS-TOKEN>/' });
const address = await client.resolveNameServiceAddress({ name: 'example.sui' });
console.log(address);
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
    let address = sui.read_api().resolve_name_service_address("example.sui").await?;
    println!("{:?}", address);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
