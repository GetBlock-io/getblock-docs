---
description: >-
  Example code for the getBlock JSON-RPC method. Complete guide on how to use
  getBlock JSON-RPC in GetBlock Web3 documentation.
---

# getBlock - Solana

This method returns identity and transaction information for a confirmed block at a given slot. Response size can be reduced with the transactionDetails and rewards options.

## Parameters

| Parameter | Type   | Required | Description                                                |
| --------- | ------ | -------- | ---------------------------------------------------------- |
| slot      | number | Yes      | Slot number of the block to fetch                          |
| config    | object | No       | Configuration object controlling encoding and detail level |

### Config Object

| Field                          | Type    | Required | Description                                                                            |
| ------------------------------ | ------- | -------- | -------------------------------------------------------------------------------------- |
| encoding                       | string  | No       | Transaction encoding: json, jsonParsed, base58, or base64                              |
| transactionDetails             | string  | No       | Level of detail: full, accounts, signatures, or none                                   |
| rewards                        | boolean | No       | Include the block rewards array. Defaults to true                                      |
| commitment                     | string  | No       | Commitment level: confirmed or finalized. The processed level is not supported         |
| maxSupportedTransactionVersion | number  | No       | Highest transaction version the client can decode. Set to 0 for versioned transactions |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getBlock",
    "params": [397234561, {"encoding": "jsonParsed", "transactionDetails": "signatures", "rewards": false, "maxSupportedTransactionVersion": 0}],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="@solana/web3.js" %}
{% code title="example.js" %}
```javascript
const { Connection } = require('@solana/web3.js');

const connection = new Connection('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', 'confirmed');

const block = await connection.getBlock(397234561, {
  transactionDetails: 'signatures',
  rewards: false,
  maxSupportedTransactionVersion: 0
});

console.log(block?.blockhash, block?.signatures?.length);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests

response = requests.post(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'getBlock',
        'params': [397234561, {"encoding": "jsonParsed", "transactionDetails": "signatures", "rewards": false, "maxSupportedTransactionVersion": 0}],
        'id': 'getblock.io'
    }
)

print(response.json()['result'])
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="example.rs" %}
```rust
use reqwest::Client;
use serde_json::{json, Value};

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let response = client
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "getBlock",
            "params": [397234561, {"encoding": "jsonParsed", "transactionDetails": "signatures", "rewards": false, "maxSupportedTransactionVersion": 0}],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;

    println!("Result: {}", response["result"]);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "blockHeight": 375812409,
        "blockTime": 1774267845,
        "blockhash": "9zJqLXK9pMhQZ4tGqVvUcxrKpKQ3pMSJ1nZmVeXtBqTd",
        "parentSlot": 397234560,
        "previousBlockhash": "8pQnEuNy2FTWSpMB1mQ2VtxYbFPqYWDVWtEZ4XKtx6Bs",
        "signatures": [
            "5wHu1qwD4kLwYqPmy1uDCgpiJ1qGpVYU3n5aHT6bSWUE1JzcbQCPSDLDPGrFyqmLLzQjLNPTPtRZbNbUJQNMkaWq"
        ]
    }
}
```
{% endcode %}

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| id        | string | Request identifier matching the request |
| result    | object | null                                    |

### Result Object

| Field             | Type   | Description                                                              |
| ----------------- | ------ | ------------------------------------------------------------------------ |
| blockhash         | string | Base58-encoded blockhash of this block                                   |
| previousBlockhash | string | Base58-encoded blockhash of the parent block                             |
| parentSlot        | number | Slot of the parent block                                                 |
| blockHeight       | number | null                                                                     |
| blockTime         | number | null                                                                     |
| transactions      | array  | Transaction objects, present when transactionDetails is full or accounts |
| signatures        | array  | Signature strings, present when transactionDetails is signatures         |
| rewards           | array  | Reward entries, present when rewards is true                             |

## Use Cases

* **Block Explorers**: Render block contents including transactions and rewards
* **Indexer Pipelines**: Stream blocks sequentially to build an off-chain database
* **Validator Rewards**: Read the rewards array to attribute fees and staking payouts
* **Throughput Analysis**: Count transactions per block to measure real network load
* **Reorg-Safe Reads**: Request finalized commitment when block data must not change

## Error Handling

| Error Code | Message                                                            | Description                                                                              |
| ---------- | ------------------------------------------------------------------ | ---------------------------------------------------------------------------------------- |
| -32007     | Slot was skipped, or missing due to ledger jump to recent snapshot | The slot was skipped by the leader or predates the node's snapshot                       |
| -32009     | Slot was skipped, or missing in long-term storage                  | The slot is absent from the node's long-term ledger storage                              |
| -32602     | Invalid params                                                     | Slot is negative or an unsupported transactionDetails value was supplied                 |
| -32015     | Transaction version (0) is not supported by the requesting client  | maxSupportedTransactionVersion was omitted for a block containing versioned transactions |
| -32004     | Block not available for slot                                       | The node has not yet produced or received the block                                      |
