---
description: >-
  Example code for the getBlocksWithLimit JSON-RPC method. Complete guide on how
  to use getBlocksWithLimit JSON-RPC in GetBlock Web3 documentation.
---

# getBlocksWithLimit - Solana

This method returns up to a given number of confirmed block slots starting from a slot. It is the cursor-friendly alternative to `getBlocks` when the end of the range is not known.

## Parameters

| Parameter | Type   | Required | Description                                       |
| --------- | ------ | -------- | ------------------------------------------------- |
| startSlot | number | Yes      | First slot of the range, inclusive                |
| limit     | number | Yes      | Maximum number of blocks to return, up to 500,000 |
| config    | object | No       | Configuration object controlling commitment       |

### Config Object

| Field      | Type   | Required | Description                                                                    |
| ---------- | ------ | -------- | ------------------------------------------------------------------------------ |
| commitment | string | No       | Commitment level: confirmed or finalized. The processed level is not supported |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getBlocksWithLimit",
    "params": [397234556, 5],
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

const blocks = await connection.getBlocksWithLimit(397234556, 5);

console.log(blocks);
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
        'method': 'getBlocksWithLimit',
        'params': [397234556, 5],
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
            "method": "getBlocksWithLimit",
            "params": [397234556, 5],
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

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": [
        397234556,
        397234557,
        397234558,
        397234560,
        397234561
    ]
}
```

## Response Parameters

| Parameter | Type   | Description                                                |
| --------- | ------ | ---------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                          |
| id        | string | Request identifier matching the request                    |
| result    | array  | Array of confirmed block slots, at most limit entries long |

## Use Cases

* **Cursor Pagination**: Page forward through the ledger using the last returned slot
* **Bounded Fetches**: Cap indexer batch size without computing an end slot
* **Catch-Up Loops**: Resume ingestion from the last processed slot after downtime
* **Sampling**: Pull a fixed number of recent blocks for spot analysis

## Error Handling

| Error Code | Message                                                            | Description                                                        |
| ---------- | ------------------------------------------------------------------ | ------------------------------------------------------------------ |
| -32007     | Slot was skipped, or missing due to ledger jump to recent snapshot | The slot was skipped by the leader or predates the node's snapshot |
| -32009     | Slot was skipped, or missing in long-term storage                  | The slot is absent from the node's long-term ledger storage        |
| -32602     | Invalid params                                                     | Limit exceeds 500,000 or startSlot is negative                     |
| -32603     | Internal error                                                     | Node failed to read the blockstore for the range                   |
