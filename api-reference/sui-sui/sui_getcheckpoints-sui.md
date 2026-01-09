---
description: >-
  Example code for the sui_getCheckpoints JSON-RPC method. Complete guide on how
  to use sui_getCheckpoints JSON-RPC in GetBlock Web3 documentation.
---

# sui\_getCheckpoints - Sui

This method returns a paginated list of checkpoints on the SUI network. Checkpoints are certified snapshots of blockchain state and are fundamental to SUI's consensus mechanism. This method allows efficient traversal of checkpoint history.

## Parameters

| Parameter         | Type                | Required | Description                        |
| ----------------- | ------------------- | -------- | ---------------------------------- |
| cursor            | BigInt\_for\_uint64 | No       | Paging cursor (sequence number)    |
| limit             | uint                | No       | Maximum items per page             |
| descending\_order | boolean             | Yes      | Sort order (true for newest first) |

{% hint style="info" %}
The `descending_order` parameter is required and controls whether results are returned with newest checkpoints first (`true`) or oldest first (`false`).
{% endhint %}

## Returns

| Field       | Type    | Description                 |
| ----------- | ------- | --------------------------- |
| data        | array   | Array of checkpoint objects |
| nextCursor  | string  | Cursor for next page        |
| hasNextPage | boolean | Whether more results exist  |

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
  "method": "sui_getCheckpoints",
  "params": ["1004", 4, false]
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
  method: 'sui_getCheckpoints',
  params: ['1004', 4, false]
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
    "method": "sui_getCheckpoints",
    "params": ["1004", 4, False]
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
        "method": "sui_getCheckpoints",
        "params": ["1004", 4, false]
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
    "data": [
      {
        "epoch": "5000",
        "sequenceNumber": "1005",
        "digest": "9zA7Q9Ka1ykvYjSQGhQCdCf32FZkcWNWx7L22JczXGsk",
        "networkTotalTransactions": "792385",
        "timestampMs": "1676911928",
        "transactions": ["7RudGLkQDBNJyqrptkrNU66Zd3pvq8MHVAHYz9WpBm59"]
      }
    ],
    "nextCursor": "1008",
    "hasNextPage": true
  },
  "id": "getblock.io"
}
```
{% endcode %}

## Response Parameters

| Parameter   | Type    | Description                         |
| ----------- | ------- | ----------------------------------- |
| data        | array   | Array of checkpoint summary objects |
| nextCursor  | string  | Cursor for next page                |
| hasNextPage | boolean | More checkpoints available          |

## Use Cases

* Build block explorers with checkpoint navigation
* Sync historical blockchain data
* Monitor checkpoint production rate
* Audit checkpoint sequence integrity

## Error Handling

| Error Code | Description                       |
| ---------- | --------------------------------- |
| -32602     | Invalid params - malformed cursor |
| -32603     | Internal error - node issues      |

## SDK Integration

{% tabs %}
{% tab title="Sui TypeScript SDK" %}
{% code title="sui-typescript.ts" %}
```typescript
import { SuiClient } from '@mysten/sui/client';

const client = new SuiClient({ url: 'https://go.getblock.io/<ACCESS-TOKEN>/' });
const checkpoints = await client.getCheckpoints({ cursor: '1004', limit: 4 });
console.log(checkpoints);
```
{% endcode %}
{% endtab %}

{% tab title="Sui Rust SDK" %}
{% code title="sui-rust.rs" overflow="wrap" %}
```rust
use sui_sdk::SuiClientBuilder;

#[tokio::main]
async fn main() -> Result<(), anyhow::Error> {
    let sui = SuiClientBuilder::default().build("https://go.getblock.io/<ACCESS-TOKEN>/").await?;
    let checkpoints = sui.read_api().get_checkpoints(Some("1004".into()), Some(4), false).await?;
    println!("{:?}", checkpoints);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
