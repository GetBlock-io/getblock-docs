---
description: >-
  Example code for the sui_executeTransactionBlock JSON-RPC method. Complete
  guide on how to use sui_executeTransactionBlock JSON-RPC in GetBlock Web3
  documentation.
---

# sui\_executeTransactionBlock - Sui

This method executes a signed transaction block on the SUI network and waits for the results. This is the primary method for submitting transactions, including token transfers, smart contract calls, NFT operations, and staking actions.

### Parameters

| Parameter     | Type   | Required | Description                                      | Default/Supported Values                        |
| ------------- | ------ | -------- | ------------------------------------------------ | ----------------------------------------------- |
| tx\_bytes     | string | Yes      | BCS serialized transaction data (Base64 encoded) | Valid Base64 transaction bytes                  |
| signatures    | array  | Yes      | Array of Base64 encoded signatures               | Valid signature strings                         |
| options       | object | No       | Response content options                         | See TransactionBlockResponseOptions             |
| request\_type | string | No       | Execution wait mode                              | "WaitForEffectsCert" or "WaitForLocalExecution" |

### Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "sui_executeTransactionBlock",
  "params": [
    "AAACACB...<base64_tx_bytes>",
    ["AQ7K...<base64_signature>"],
    {
      "showInput": true,
      "showEffects": true,
      "showEvents": true,
      "showObjectChanges": true,
      "showBalanceChanges": true
    },
    "WaitForLocalExecution"
  ],

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
    "method": "sui_executeTransactionBlock",
  "params": [
    "AAACACB...<base64_tx_bytes>",
    ["AQ7K...<base64_signature>"],
    {
      "showInput": true,
      "showEffects": true,
      "showEvents": true,
      "showObjectChanges": true,
      "showBalanceChanges": true
    },
    "WaitForLocalExecution"
  ],
  "id": "getblock.io"
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
     "method": "sui_executeTransactionBlock",
  "params": [
    "AAACACB...<base64_tx_bytes>",
    ["AQ7K...<base64_signature>"],
    {
      "showInput": true,
      "showEffects": true,
      "showEvents": true,
      "showObjectChanges": true,
      "showBalanceChanges": true
    },
    "WaitForLocalExecution"
  ],
"id": "getblock.io"
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
            "method": "sui_executeTransactionBlock",
          "params": [
            "AAACACB...<base64_tx_bytes>",
            ["AQ7K...<base64_signature>"],
            {
              "showInput": true,
              "showEffects": true,
              "showEvents": true,
              "showObjectChanges": true,
              "showBalanceChanges": true
            },
            "WaitForLocalExecution"
          ],

            "id": "getblock.io"
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

### **Response**

```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": {
    "digest": "7dp5WtTmtGp83EXYYFMzjBJRFeSgR67AzqMETLrfgeFx",
    "effects": {
      "messageVersion": "v1",
      "status": {"status": "success"},
      "executedEpoch": "100",
      "gasUsed": {
        "computationCost": "1000000",
        "storageCost": "2000000",
        "storageRebate": "500000"
      }
    },
    "events": [],
    "objectChanges": [],
    "balanceChanges": [],
    "confirmedLocalExecution": true
  }
}
```

### Repsonse Parameters

| Parameter                      | Type    | Description                           |
| ------------------------------ | ------- | ------------------------------------- |
| result.digest                  | string  | Unique transaction digest             |
| result.transaction             | object  | Executed transaction data             |
| result.effects                 | object  | Execution results and gas usage       |
| result.events                  | array   | Events emitted during execution       |
| result.objectChanges           | array   | Objects created, modified, or deleted |
| result.balanceChanges          | array   | Balance changes per address           |
| result.confirmedLocalExecution | boolean | Local execution confirmation          |

### Use Cases

1. **Token Transfers:** Submit signed transactions to transfer SUI or custom tokens between addresses on the network.
2. **Smart Contract Execution:** Execute Move function calls on deployed packages, enabling interaction with DeFi protocols and dApps.
3. **NFT Operations:** Mint, transfer, or burn NFTs by executing the appropriate transaction blocks.

### Error handling

| Error                   | Description                   | Solution                                   |
| ----------------------- | ----------------------------- | ------------------------------------------ |
| InvalidSignature        | Signature verification failed | Ensure the transaction is signed correctly |
| InsufficientGas         | Gas budget insufficient       | Increase gas budget in transaction         |
| InsufficientCoinBalance | Not enough balance            | Verify sender has sufficient funds         |
| ObjectNotFound          | Referenced object missing     | Check object IDs are valid                 |

### SDK Integration

{% tabs %}
{% tab title="Sui TypeScript SDK" %}
{% code title="example.ts" %}
```typescript
import { SuiClient } from '@mysten/sui/client';
import { Ed25519Keypair } from '@mysten/sui/keypairs/ed25519';
import { Transaction } from '@mysten/sui/transactions';

const client = new SuiClient({ url: 'https://go.getblock.io/<ACCESS-TOKEN>/' });
const keypair = new Ed25519Keypair();

const tx = new Transaction();
tx.transferObjects([tx.object('0x...')], tx.pure.address('0x...'));

const result = await client.signAndExecuteTransaction({
  signer: keypair,
  transaction: tx,
  options: { showEffects: true, showEvents: true },
});

console.log(result);
```
{% endcode %}
{% endtab %}

{% tab title="Sui Rust SDK" %}
{% code title="example_sui_rust.rs" %}
```rust
use sui_sdk::SuiClientBuilder;

#[tokio::main]
async fn main() -> Result<(), anyhow::Error> {
    let sui = SuiClientBuilder::default()
        .build("https://go.getblock.io/<ACCESS-TOKEN>/")
        .await?;
    
    // Build and sign transaction, then execute
    let result = sui.quorum_driver_api()
        .execute_transaction_block(tx_bytes, vec![signature], None, None)
        .await?;
    
    println!("{:?}", result);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
