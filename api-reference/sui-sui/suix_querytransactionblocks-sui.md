---
description: >-
  Example code for the suix_queryTransactionBlocks JSON-RPC method. Complete
  guide on how to use suix_queryTransactionBlocks JSON-RPC in GetBlock Web3
  documentation.
---

# suix\_queryTransactionBlocks - Sui

This method returns transactions matching specified query criteria on the SUI network. This query supports filtering by sender, recipient, input objects, changed objects, and Move function calls with pagination support.

## Parameters

| Parameter         | Type                   | Required | Description                            |
| ----------------- | ---------------------- | -------- | -------------------------------------- |
| query             | TransactionBlocksQuery | Yes      | Query criteria with filter and options |
| cursor            | TransactionDigest      | No       | Pagination cursor                      |
| limit             | uint                   | No       | Maximum items per page                 |
| descending\_order | boolean                | No       | Sort order (default: false)            |

### Filter Options

* `Checkpoint` - Filter by checkpoint
* `MoveFunction` - Filter by Move function call
* `InputObject` - Filter by input object
* `ChangedObject` - Filter by modified object
* `FromAddress` - Filter by sender
* `ToAddress` - Filter by recipient
* `FromAndToAddress` - Filter by both sender and recipient
* `TransactionKind` - Filter by transaction type

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "suix_queryTransactionBlocks",
  "params": [
    {
      "filter": {
        "InputObject": "0x93633829fcba6d6e0ccb13d3dbfe7614b81ea76b255e5d435032cd8595f37eb8"
      },
      "options": null
    },
    null,
    100,
    false
  ]
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
const axios = require('axios');

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'suix_queryTransactionBlocks',
  params: [
    { filter: { InputObject: '0x93633829fcba6d6e0ccb13d3dbfe7614b81ea76b255e5d435032cd8595f37eb8' }, options: null },
    null,
    100,
    false
  ]
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
    "method": "suix_queryTransactionBlocks",
    "params": [
        {
            "filter": {"InputObject": "0x93633829fcba6d6e0ccb13d3dbfe7614b81ea76b255e5d435032cd8595f37eb8"},
            "options": None
        },
        None,
        100,
        False
    ]
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
        "method": "suix_queryTransactionBlocks",
        "params": [
            {"filter": {"InputObject": "0x93633829..."}, "options": null},
            null, 100, false
        ]
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
    "data": [
      {
        "digest": "5PLgmQye6rraDYqpV3npV6H1cUXoJZgJh1dPCyRa3WCv",
        "timestampMs": "1676911928000",
        "checkpoint": "1000"
      }
    ],
    "nextCursor": "5PLgmQye6rraDYqpV3npV6H1cUXoJZgJh1dPCyRa3WCv",
    "hasNextPage": false
  },
  "id": "getblock.io"
}
```

## Response Parameters

| Parameter   | Type    | Description                  |
| ----------- | ------- | ---------------------------- |
| data        | array   | Matching transaction digests |
| nextCursor  | string  | Cursor for pagination        |
| hasNextPage | boolean | More results available       |

## Use Cases

* Query transaction history by address
* Find transactions involving specific objects
* Track Move function calls
* Build transaction explorers
* Monitor contract interactions

## Error Handling

| Error Code | Description                      |
| ---------- | -------------------------------- |
| -32602     | Invalid params - malformed query |
| -32603     | Internal error - node issues     |

## SDK Integration

{% tabs %}
{% tab title="Sui TypeScript SDK" %}
```typescript
import { SuiClient } from '@mysten/sui/client';

const client = new SuiClient({ url: 'https://go.getblock.io/<ACCESS-TOKEN>/' });
const txs = await client.queryTransactionBlocks({
  filter: { InputObject: '0x93633829fcba6d6e0ccb13d3dbfe7614b81ea76b255e5d435032cd8595f37eb8' },
  limit: 100
});
console.log(txs);
```
{% endtab %}

{% tab title="Sui Rust SDK" %}
```rust
use sui_sdk::SuiClientBuilder;
use sui_sdk::rpc_types::TransactionFilter;

#[tokio::main]
async fn main() -> Result<(), anyhow::Error> {
    let sui = SuiClientBuilder::default().build("https://go.getblock.io/<ACCESS-TOKEN>/").await?;
    let txs = sui.read_api().query_transaction_blocks(
        TransactionFilter::InputObject("0x93633829...".parse()?),
        None, None, false
    ).await?;
    println!("{:?}", txs);
    Ok(())
}
```
{% endtab %}
{% endtabs %}
