---
description: >-
  Example code for the getBlocks JSON-RPC method. Complete guide on how to use
  getBlocks JSON-RPC in GetBlock Web3 documentation.
---

# getBlocks - Solana

This method returns the confirmed block slots in an inclusive slot range. The range is capped at 500,000 slots per request.

## Parameters

| Parameter | Type   | Required | Description                                                                           |
| --------- | ------ | -------- | ------------------------------------------------------------------------------------- |
| startSlot | number | Yes      | First slot of the range, inclusive                                                    |
| endSlot   | number | No       | Last slot of the range, inclusive. Must be no more than 500,000 slots after startSlot |
| config    | object | No       | Configuration object controlling commitment                                           |

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
    "method": "getBlocks",
    "params": [397234556, 397234561],
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

const blocks = await connection.getBlocks(397234556, 397234561);

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
        'method': 'getBlocks',
        'params': [397234556, 397234561],
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
            "method": "getBlocks",
            "params": [397234556, 397234561],
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

| Parameter | Type   | Description                                           |
| --------- | ------ | ----------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                     |
| id        | string | Request identifier matching the request               |
| result    | array  | Array of confirmed block slots in the requested range |

## Use Cases

* **Gap Detection**: Compare the returned slots against the range to find skipped slots
* **Indexer Scheduling**: Enumerate slots to fetch before calling getBlock on each
* **Backfill Jobs**: Chunk historical ranges into 500,000-slot windows
* **Liveness Checks**: Measure how many slots in a window produced blocks

## Error Handling

| Error Code | Message                                                            | Description                                                        |
| ---------- | ------------------------------------------------------------------ | ------------------------------------------------------------------ |
| -32007     | Slot was skipped, or missing due to ledger jump to recent snapshot | The slot was skipped by the leader or predates the node's snapshot |
| -32009     | Slot was skipped, or missing in long-term storage                  | The slot is absent from the node's long-term ledger storage        |
| -32602     | Invalid params                                                     | Range exceeds 500,000 slots or endSlot precedes startSlot          |
| -32603     | Internal error                                                     | Node failed to read the blockstore for the range                   |
