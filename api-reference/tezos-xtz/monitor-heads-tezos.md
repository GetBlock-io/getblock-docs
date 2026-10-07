---
description: >-
  Example code for the /monitor/heads/main REST method. Complete guide on how to
  use the /monitor/heads/main REST method in GetBlock Web3 documentation
---

# monitor-heads - Tezos

Opens a streaming response that emits each new block header as it is appended to the main chain. Use it to react to new blocks in real time. This is a long-lived monitor endpoint.

## Endpoint

```http
GET /monitor/heads/main
```

## Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
export TEZOS_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${TEZOS_REST}monitor/heads/main"
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/monitor/heads/main', {
    headers: { 'Content-Type': 'application/json' },
    responseType: 'stream'
});

response.data.on('data', chunk => {
    console.log(chunk.toString());
});
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests

response = requests.get(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/monitor/heads/main',
    headers={'Content-Type': 'application/json'},
    stream=True
)

for line in response.iter_lines():
    if line:
        print(line.decode())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="example.rs" %}
```rust
use reqwest::Client;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let mut response = client
        .get("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/monitor/heads/main")
        .header("Content-Type", "application/json")
        .send()
        .await?;

    while let Some(chunk) = response.chunk().await? {
        println!("{}", String::from_utf8_lossy(&chunk));
    }

    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "hash": "BMblockhash...",
    "level": 5000000,
    "proto": 20,
    "predecessor": "BMprev...",
    "timestamp": "2025-01-15T12:00:00Z",
    "operations_hash": "LLoa...",
    "fitness": [
        "02",
        "..."
    ]
}
```

## Response Fields

| Field    | Type   | Description                          |
| -------- | ------ | ------------------------------------ |
| (stream) | object | One block header object per new head |

## Use Cases

* **Real-Time Feeds**: Trigger logic on every new block
* **Confirmation Watching**: Detect when a transaction's block is produced

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path or body parameter was invalid              |
| 404 / Not found           | Not found     | The block, contract, or resource does not exist   |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
