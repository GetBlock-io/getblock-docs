---
description: >-
  Example code for the sui_dryRunTransactionBlock JSON-RPC method. Complete
  guide on how to use sui_dryRunTransactionBlock JSON-RPC in GetBlock Web3
  documentation.
---

# sui\_dryRunTransactionBlock - Sui

This method performs a dry run of a transaction block without committing it to the blockchain. This simulates transaction execution to predict effects, gas costs, and potential errors before actual submission. It's essential for transaction validation, gas estimation, and debugging.

## Parameters

| Parameter | Type   | Required | Description                           |
| --------- | ------ | -------- | ------------------------------------- |
| tx\_bytes | Base64 | Yes      | BCS serialized transaction data bytes |

## Returns

| Field          | Type   | Description                      |
| -------------- | ------ | -------------------------------- |
| effects        | object | Simulated execution effects      |
| events         | array  | Events that would be emitted     |
| objectChanges  | array  | Objects that would be modified   |
| balanceChanges | array  | Balance changes that would occur |
| input          | object | Parsed transaction input         |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="curl" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "sui_dryRunTransactionBlock",
  "params": ["AAACACB...<base64_tx_bytes>"]
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="request.js" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'sui_dryRunTransactionBlock',
  params: ['AAACACB...<base64_tx_bytes>']
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
{% code title="request.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "sui_dryRunTransactionBlock",
    "params": ["AAACACB...<base64_tx_bytes>"]
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
        "method": "sui_dryRunTransactionBlock",
        "params": ["AAACACB...<base64_tx_bytes>"]
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

{% code title="response.json" %}
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
        "storageRebate": "500000",
        "nonRefundableStorageFee": "0"
      }
    },
    "events": [],
    "objectChanges": [],
    "balanceChanges": [],
    "input": {}
  },
  "id": "getblock.io"
}
```
{% endcode %}

## Response Parameters

| Parameter      | Type   | Description                                    |
| -------------- | ------ | ---------------------------------------------- |
| effects        | object | Simulated transaction effects and gas usage    |
| events         | array  | Events that would be emitted                   |
| objectChanges  | array  | Objects that would be created/modified/deleted |
| balanceChanges | array  | Balance changes for each affected address      |
| input          | object | Parsed transaction input data                  |

## Use Cases

* Estimate gas costs before submission
* Validate transaction parameters
* Debug failed transaction logic
* Preview object and balance changes
* Test smart contract interactions safely

## Error Handling

| Error Code      | Description                                  |
| --------------- | -------------------------------------------- |
| -32602          | Invalid params - malformed transaction bytes |
| -32603          | Internal error - simulation failed           |
| MoveAbort       | Move execution aborted with error code       |
| InsufficientGas | Gas budget would be insufficient             |

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

const dryRunResult = await client.dryRunTransactionBlock({
  transactionBlock: await tx.build({ client }),
});

console.log(dryRunResult);
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
    
    // let tx_bytes = ...
    let result = sui.read_api().dry_run_transaction_block(tx_bytes).await?;
    
    println!("{:?}", result);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
