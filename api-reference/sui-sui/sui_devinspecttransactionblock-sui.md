---
description: >-
  Example code for the sui_devInspectTransactionBlock JSON-RPC method. Complete
  guide on how to use sui_devInspectTransactionBlock JSON-RPC in GetBlock Web3
  documentation.
---

# sui\_devInspectTransactionBlock - Sui

This method runs a transaction in dev-inspect mode for testing and development purposes. Unlike `dryRunTransactionBlock`, this method, it doesn't require gas payment and can simulate transactions from any sender address. It's primarily used for debugging and testing Move function calls.

## Parameters

| Parameter       | Type                | Required | Description                    |
| --------------- | ------------------- | -------- | ------------------------------ |
| sender\_address | SuiAddress          | Yes      | The simulated sender's address |
| tx\_bytes       | Base64              | Yes      | BCS encoded TransactionKind    |
| gas\_price      | BigInt\_for\_uint64 | No       | Gas price for cost calculation |
| epoch           | BigInt\_for\_uint64 | No       | Epoch to simulate execution in |

## Returns

| Field   | Type   | Description                   |
| ------- | ------ | ----------------------------- |
| effects | object | Simulated execution effects   |
| results | array  | Return values from Move calls |
| events  | array  | Events that would be emitted  |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "sui_devInspectTransactionBlock",
  "params": [
    "0x94f1a597b4e8f709a396f7f6b1482bdcd65a673d111e49286c527fab7c2d0961",
    "AAACACB...<base64_tx_bytes>",
    null,
    null
  ]
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'sui_devInspectTransactionBlock',
  params: [
    '0x94f1a597b4e8f709a396f7f6b1482bdcd65a673d111e49286c527fab7c2d0961',
    'AAACACB...<base64_tx_bytes>',
    null,
    null
  ]
};

axios.post(url, payload, {
  headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log(response.data))
.catch(error => console.error(error));
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "sui_devInspectTransactionBlock",
    "params": [
        "0x94f1a597b4e8f709a396f7f6b1482bdcd65a673d111e49286c527fab7c2d0961",
        "AAACACB...<base64_tx_bytes>",
        None,
        None
    ]
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
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
        "method": "sui_devInspectTransactionBlock",
        "params": [
            "0x94f1a597b4e8f709a396f7f6b1482bdcd65a673d111e49286c527fab7c2d0961",
            "AAACACB...<base64_tx_bytes>",
            null,
            null
        ]
    });

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&payload)
        .send()
        .await?;

    let result: serde_json::Value = response.json().await?;
    println!("{:#?}", result);
    
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
    "effects": {
      "messageVersion": "v1",
      "status": {
        "status": "success"
      },
      "gasUsed": {
        "computationCost": "1000000",
        "storageCost": "2000000",
        "storageRebate": "500000"
      }
    },
    "results": [
      {
        "returnValues": [],
        "mutableReferenceOutputs": []
      }
    ],
    "events": []
  },
  "id": "getblock.io"
}
```

## Response Parameters

| Parameter | Type   | Description                               |
| --------- | ------ | ----------------------------------------- |
| effects   | object | Simulated execution effects and gas costs |
| results   | array  | Return values from Move function calls    |
| events    | array  | Events that would be emitted              |

## Use Cases

* Test Move function calls without gas
* Debug smart contract logic
* Simulate transactions from any address
* Inspect return values from functions
* Development and testing workflows

## Error Handling

| Error Code | Description                        |
| ---------- | ---------------------------------- |
| -32602     | Invalid params - malformed input   |
| -32603     | Internal error - simulation failed |
| MoveAbort  | Move execution aborted             |

## SDK Integration

{% tabs %}
{% tab title="Sui TypeScript SDK" %}
{% code title="example.ts" %}
```typescript
import { SuiClient } from '@mysten/sui/client';
import { Transaction } from '@mysten/sui/transactions';

const client = new SuiClient({ url: 'https://go.getblock.io/<ACCESS-TOKEN>/' });

const tx = new Transaction();
// ... build transaction

const result = await client.devInspectTransactionBlock({
  sender: '0x94f1a597b4e8f709a396f7f6b1482bdcd65a673d111e49286c527fab7c2d0961',
  transactionBlock: tx,
});

console.log(result);
```
{% endcode %}
{% endtab %}

{% tab title="Sui Rust SDK" %}
{% code title="main.rs" %}
```rust
use sui_sdk::SuiClientBuilder;

#[tokio::main]
async fn main() -> Result<(), anyhow::Error> {
    let sui = SuiClientBuilder::default()
        .build("https://go.getblock.io/<ACCESS-TOKEN>/")
        .await?;
    
    let sender = "0x94f1a597b4e8f709a396f7f6b1482bdcd65a673d111e49286c527fab7c2d0961".parse()?;
    // let tx_bytes = ...
    let result = sui.read_api().dev_inspect_transaction_block(sender, tx_bytes, None, None).await?;
    
    println!("{:?}", result);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
