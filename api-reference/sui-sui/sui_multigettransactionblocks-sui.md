---
description: >-
  Example code for the sui_multiGetTransactionBlocks JSON-RPC method. Complete
  guide on how to use sui_multiGetTransactionBlocks JSON-RPC in GetBlock Web3
  documentation.
---

# sui\_multiGetTransactionBlocks - Sui

The `sui_multiGetTransactionBlocks` method returns transaction block data for a list of transaction digests on the SUI network in a single request. This batch method is more efficient than making individual `sui_getTransactionBlock` calls when you need data for multiple transactions.

## Parameters

| Parameter | Type                            | Required | Description                           |
| --------- | ------------------------------- | -------- | ------------------------------------- |
| digests   | array                           | Yes      | Array of transaction digests to query |
| options   | TransactionBlockResponseOptions | No       | Options for response content          |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "sui_multiGetTransactionBlocks",
  "params": [
    [
      "5PLgmQye6rraDYqpV3npV6H1cUXoJZgJh1dPCyRa3WCv",
      "7dp5WtTmtGp83EXYYFMzjBJRFeSgR67AzqMETLrfgeFx"
    ],
    {
      "showInput": true,
      "showEffects": true
    }
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
  method: 'sui_multiGetTransactionBlocks',
  params: [
    [
      '5PLgmQye6rraDYqpV3npV6H1cUXoJZgJh1dPCyRa3WCv',
      '7dp5WtTmtGp83EXYYFMzjBJRFeSgR67AzqMETLrfgeFx'
    ],
    { showInput: true, showEffects: true }
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
    "method": "sui_multiGetTransactionBlocks",
    "params": [
        [
            "5PLgmQye6rraDYqpV3npV6H1cUXoJZgJh1dPCyRa3WCv",
            "7dp5WtTmtGp83EXYYFMzjBJRFeSgR67AzqMETLrfgeFx"
        ],
        {"showInput": True, "showEffects": True}
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
        "method": "sui_multiGetTransactionBlocks",
        "params": [
            ["5PLgmQye6rraDYqpV3npV6H1cUXoJZgJh1dPCyRa3WCv"],
            {"showInput": true, "showEffects": true}
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
  "result": [
    {
      "digest": "5PLgmQye6rraDYqpV3npV6H1cUXoJZgJh1dPCyRa3WCv",
      "transaction": {
        "data": {
          "messageVersion": "v1",
          "transaction": { "kind": "ProgrammableTransaction" },
          "sender": "0xc8ec1d5b84dd6289e193b9f88de4a994358c9f856135236c3e75a925e1c77ac3"
        }
      },
      "effects": {
        "status": { "status": "success" }
      }
    }
  ],
  "id": "getblock.io"
}
```

## Response Parameters

| Parameter | Type  | Description                          |
| --------- | ----- | ------------------------------------ |
| result    | array | Array of transaction block responses |

## Use Cases

* Fetch transaction history efficiently
* Load multiple transactions for analysis
* Build transaction explorers
* Batch verify transaction statuses

## Error Handling

| Error Code | Description                        |
| ---------- | ---------------------------------- |
| -32602     | Invalid params - malformed digests |
| -32603     | Internal error - node issues       |

## SDK Integration

{% tabs %}
{% tab title="Sui TypeScript SDK" %}
```typescript
import { SuiClient } from '@mysten/sui/client';

const client = new SuiClient({ url: 'https://go.getblock.io/<ACCESS-TOKEN>/' });
const txs = await client.multiGetTransactionBlocks({
  digests: ['5PLgmQye6rraDYqpV3npV6H1cUXoJZgJh1dPCyRa3WCv'],
  options: { showInput: true, showEffects: true }
});
console.log(txs);
```
{% endtab %}

{% tab title="Sui Rust SDK" %}
{% code overflow="wrap" %}
```rust
use sui_sdk::SuiClientBuilder;

#[tokio::main]
async fn main() -> Result<(), anyhow::Error> {
    let sui = SuiClientBuilder::default().build("https://go.getblock.io/<ACCESS-TOKEN>/").await?;
    let digests = vec!["5PLgmQye6rraDYqpV3npV6H1cUXoJZgJh1dPCyRa3WCv".parse()?];
    let txs = sui.read_api().multi_get_transactions_with_options(digests, None).await?;
    println!("{:?}", txs);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
