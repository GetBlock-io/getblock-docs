---
description: >-
  Example code for the sui_getTransactionBlock JSON-RPC method. Complete guide
  on how to use sui_getTransactionBlock JSON-RPC in GetBlock Web3 documentation.
---

# sui\_getTransactionBlock - Sui

This method retrieves detailed information about a transaction block for a specified digest on the SUI network, including inputs, effects, events, object changes, and balance changes.

### Parameters

| Parameter | Type   | Required | Description                                  | Default/Supported Values            |
| --------- | ------ | -------- | -------------------------------------------- | ----------------------------------- |
| digest    | string | Yes      | The transaction digest (Base58 encoded hash) | Valid transaction digest string     |
| options   | object | No       | Options specifying which data to include     | See TransactionBlockResponseOptions |

TransactionBlockResponseOptions

| Field              | Type    | Default | Description                    |
| ------------------ | ------- | ------- | ------------------------------ |
| showInput          | boolean | false   | Include transaction input data |
| showRawInput       | boolean | false   | Include raw transaction bytes  |
| showEffects        | boolean | false   | Include execution effects      |
| showEvents         | boolean | false   | Include emitted events         |
| showObjectChanges  | boolean | false   | Include object modifications   |
| showBalanceChanges | boolean | false   | Include balance changes        |

### Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/ \
--header 'Content-Type: application/json' \
--data-raw '{
  "jsonrpc": "2.0",
  "method": "sui_getTransactionBlock",
  "params": [
    "5PLgmQye6rraDYqpV3npV6H1cUXoJZgJh1dPCyRa3WCv",
    {
      "showInput": true,
      "showEffects": true,
      "showEvents": true,
      "showObjectChanges": true,
      "showBalanceChanges": true
    }
  ],
  "id": "getblock.io"
}'
```
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
        "method": "sui_getTransactionBlock",
        "params": [
            "5PLgmQye6rraDYqpV3npV6H1cUXoJZgJh1dPCyRa3WCv",
            {
                "showInput": true,
                "showEffects": true,
                "showEvents": true
            }
        ]
    });

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&payload)
        .send()
        .await?;

    println!("Response: {:?}", response.text().await?);
    Ok(())
}
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="get_transaction.js" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/";
const payload = {
  jsonrpc: "2.0",
  method: "sui_getTransactionBlock",
  params: [
    "5PLgmQye6rraDYqpV3npV6H1cUXoJZgJh1dPCyRa3WCv",
    {
      showInput: true,
      showEffects: true,
      showEvents: true
    }
  ],
  id: "getblock.io"
};

axios.post(url, payload, {
  headers: { "Content-Type": "application/json" }
})
.then(response => {
  console.log("Result:", response.data.result);
})
.catch(error => {
  console.error("Error:", error.response?.status, error.response?.data);
});
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="get_transaction.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"
headers = {"Content-Type": "application/json"}

payload = {
    "jsonrpc": "2.0",
    "method": "sui_getTransactionBlock",
    "params": [
        "5PLgmQye6rraDYqpV3npV6H1cUXoJZgJh1dPCyRa3WCv",
        {
            "showInput": True,
            "showEffects": True,
            "showEvents": True
        }
    ],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

if response.status_code == 200:
    print("Result:", response.json().get("result"))
else:
    print("Error:", response.status_code, response.text)
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Response

```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": {
    "digest": "5PLgmQye6rraDYqpV3npV6H1cUXoJZgJh1dPCyRa3WCv",
    "transaction": {
      "data": {
        "messageVersion": "v1",
        "transaction": {
          "kind": "ProgrammableTransaction",
          "inputs": [],
          "commands": []
        },
        "sender": "0xc8ec1d5b84dd6289e193b9f88de4a994358c9f856135236c3e75a925e1c77ac3",
        "gasData": {
          "payment": [],
          "owner": "0xc8ec1d5b84dd6289e193b9f88de4a994358c9f856135236c3e75a925e1c77ac3",
          "price": "1000",
          "budget": "10000000"
        }
      }
    },
    "effects": {
      "messageVersion": "v1",
      "status": {"status": "success"},
      "executedEpoch": "0",
      "gasUsed": {
        "computationCost": "1000000",
        "storageCost": "2000000",
        "storageRebate": "500000"
      }
    },
    "events": [],
    "objectChanges": [],
    "balanceChanges": [],
    "timestampMs": "1676911928000",
    "checkpoint": "1000"
  }
}
```

### Repsonse Parameters

| Parameter             | Type   | Description                                          |
| --------------------- | ------ | ---------------------------------------------------- |
| result.digest         | string | Transaction digest (unique identifier)               |
| result.transaction    | object | Transaction input data including sender and gas info |
| result.effects        | object | Execution results, status, and gas usage             |
| result.events         | array  | Events emitted during execution                      |
| result.objectChanges  | array  | Objects created, mutated, or deleted                 |
| result.balanceChanges | array  | Balance changes per affected address                 |
| result.timestampMs    | string | Transaction timestamp in milliseconds                |
| result.checkpoint     | string | Checkpoint sequence number                           |

***

### Use Cases

1. Transaction Verification: Verify transaction execution status and results, confirming successful completion or diagnosing failures in dApp workflows.
2. Block Explorer Development: Build block explorer functionality by displaying comprehensive transaction details, including inputs, effects, and state changes.
3. Event Monitoring: Track smart contract events emitted during transaction execution for real-time monitoring and notifications.

### Error Handling&#x20;

| Code         | Description                         | Solution                                                |
| ------------ | ----------------------------------- | ------------------------------------------------------- |
| -32700       | The transaction digest is malformed | Ensure the digest is a valid Base58 encoded string      |
| -32602       |  Transaction not found              | Verify the digest is correct and the transaction exists |
| Network Sync | Node not fully synced               | Wait for node synchronization or try another endpoint   |

### SDK Integration

{% tabs %}
{% tab title="Sui TypeScript SDK" %}
{% code title="sui-client.ts" %}
```typescript
import { SuiClient } from '@mysten/sui/client';

const client = new SuiClient({ url: 'https://go.getblock.io/<ACCESS-TOKEN>/' });

const tx = await client.getTransactionBlock({
  digest: '5PLgmQye6rraDYqpV3npV6H1cUXoJZgJh1dPCyRa3WCv',
  options: {
    showInput: true,
    showEffects: true,
    showEvents: true,
  },
});

console.log(tx);
```
{% endcode %}
{% endtab %}

{% tab title="Sui Rust SDK" %}
{% code title="sui_sdk_example.rs" overflow="wrap" %}
```rust
use sui_sdk::SuiClientBuilder;
use sui_types::digests::TransactionDigest;

#[tokio::main]
async fn main() -> Result<(), anyhow::Error> {
    let sui = SuiClientBuilder::default()
        .build("https://go.getblock.io/<ACCESS-TOKEN>/")
        .await?;
    
    let digest: TransactionDigest = "5PLgmQye6rraDYqpV3npV6H1cUXoJZgJh1dPCyRa3WCv".parse()?;
    let tx = sui.read_api().get_transaction_with_options(digest, None).await?;
    
    println!("{:?}", tx);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

