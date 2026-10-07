---
description: >-
  Example code for the /v1/node_version_info REST method. Complete guide on how
  to use the /v1/node_version_info REST method in GetBlock Web3 documentation
---

# node-version-info - Flow

Returns version information about the Access node serving the request, including the protocol version, spork ID, and node role. Use it to confirm the node build and current spork.

## Endpoint

```http
GET /v1/node_version_info
```

## Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
export FLOW_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${FLOW_REST}v1/node_version_info"
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/v1/node_version_info', {
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/v1/node_version_info',
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
        .get("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/v1/node_version_info")
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
    "semver": "v0.37.0",
    "commit": "abcd1234",
    "spork_id": "mainnet-24",
    "protocol_version": "0",
    "spork_root_block_height": "88226267",
    "node_root_block_height": "88226267"
}
```

## Response Fields

| Field                     | Type   | Description                         |
| ------------------------- | ------ | ----------------------------------- |
| semver                    | string | Access node semantic version        |
| spork\_id                 | string | Current spork identifier            |
| node\_root\_block\_height | string | First block height this node serves |

## Use Cases

* **Node Identification**: Confirm the Access node build behind an endpoint
* **Spork Awareness**: Detect the active spork before historical queries

## Error Handling

| Error                     | Message       | Description                                                 |
| ------------------------- | ------------- | ----------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                |
| 404 / Not found           | Not found     | The block, account, transaction, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect           |
