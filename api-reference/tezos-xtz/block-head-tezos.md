---
description: >-
  Example code for the /chains/main/blocks/head REST method. Complete guide on
  how to use the /chains/main/blocks/head REST method in GetBlock Web3
  documentation
---

# block-head - Tezos

Returns the complete block at the chain head — its hash, header, metadata, and all operations. Replace head with a level or block hash to fetch any block.

## Endpoint

```http
GET /chains/main/blocks/head
```

## Path Parameters

| Parameter | Type   | Description                                                     |
| --------- | ------ | --------------------------------------------------------------- |
| block     | string | Block identifier: head, a level (e.g. 5000000), or a block hash |

## Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
export TEZOS_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${TEZOS_REST}chains/main/blocks/head"
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head', {
    headers: { 'Content-Type': 'application/json' }
});

console.log(response.data);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests

response = requests.get(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head',
    headers={'Content-Type': 'application/json'}
)

print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="example.rs" %}
```rust
use reqwest::Client;
use serde_json::Value;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let response = client
        .get("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head")
        .header("Content-Type", "application/json")
        .send()
        .await?
        .json::<Value>()
        .await?;

    println!("Result: {}", response);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "protocol": "PsParisC...",
    "chain_id": "NetXdQprcVkpaWU",
    "hash": "BMblockhash...",
    "header": {
        "level": 5000000,
        "proto": 20,
        "predecessor": "BMprev...",
        "timestamp": "2025-01-15T12:00:00Z"
    },
    "metadata": {
        "protocol": "PsParisC..."
    },
    "operations": [
        [],
        [],
        [],
        []
    ]
}
```

## Response Fields

| Field      | Type   | Description                           |
| ---------- | ------ | ------------------------------------- |
| protocol   | string | Active protocol hash                  |
| hash       | string | Block hash                            |
| header     | object | Shell + protocol header               |
| metadata   | object | Block metadata                        |
| operations | array  | Operations grouped by validation pass |

## Use Cases

* **Block Inspection**: Read a full block by level or hash
* **Indexing**: Ingest blocks for an explorer or analytics pipeline

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path or body parameter was invalid              |
| 404 / Not found           | Not found     | The block, contract, or resource does not exist   |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
