---
description: >-
  Example code for the /v1/events REST method. Complete guide on how to use the
  /v1/events REST method in GetBlock Web3 documentation
---

# events - Flow

Returns events of a given type within a block-height range, or across a set of block IDs. Event type strings follow the `A.{address}.{contract}.{event}` format. Use it to index on-chain activity such as token transfers.

## Endpoint

```http
GET /v1/events
```

## Query Parameters

| Parameter     | Type   | Description                                                                |
| ------------- | ------ | -------------------------------------------------------------------------- |
| type          | string | Event type, e.g. `A.1654653399040a61.FlowToken.TokensDeposited` (required) |
| start\_height | string | Start block height of the range                                            |
| end\_height   | string | End block height of the range                                              |
| block\_ids    | string | Comma-separated block IDs instead of a range                               |

## Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
export FLOW_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${FLOW_REST}v1/events?type=A.1654653399040a61.FlowToken.TokensDeposited&start_height=88226300&end_height=88226301"
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/v1/events', {
    params: {
        type: 'A.1654653399040a61.FlowToken.TokensDeposited',
        start_height: '88226300',
        end_height: '88226301'
    },
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/v1/events',
    headers={'Content-Type': 'application/json'},
    params={
        'type': 'A.1654653399040a61.FlowToken.TokensDeposited',
        'start_height': '88226300',
        'end_height': '88226301'
    }
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
        .get("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/v1/events")
        .query(&[
            ("type", "A.1654653399040a61.FlowToken.TokensDeposited"),
            ("start_height", "88226300"),
            ("end_height", "88226301"),
        ])
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
[
    {
        "block_id": "3b5f...",
        "block_height": "88226300",
        "block_timestamp": "2025-01-15T12:00:00Z",
        "events": [
            {
                "type": "A.1654653399040a61.FlowToken.TokensDeposited",
                "transaction_id": "a1b2...",
                "transaction_index": "0",
                "event_index": "0",
                "payload": "eyJ0eXBlIjoi..."
            }
        ]
    }
]
```

## Response Fields

| Field         | Type   | Description                                          |
| ------------- | ------ | ---------------------------------------------------- |
| block\_height | string | Height the events were emitted at                    |
| events        | array  | Events with type, transaction id, and base64 payload |

## Use Cases

* **Activity Indexing**: Track token transfers and NFT events
* **Analytics**: Aggregate contract events over a height range

## Error Handling

| Error                     | Message       | Description                                                 |
| ------------------------- | ------------- | ----------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                |
| 404 / Not found           | Not found     | The block, account, transaction, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect           |
