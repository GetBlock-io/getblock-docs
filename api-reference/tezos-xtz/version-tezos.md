---
description: >-
  Example code for the /version REST method. Complete guide on how to use the
  /version REST method in GetBlock Web3 documentation
---

# version - Tezos

Returns the Octez node version, network protocol, and commit information. Use it to confirm the node build serving a GetBlock endpoint.

## Endpoint

```http
GET /version
```

## Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
export TEZOS_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${TEZOS_REST}version"
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/version', {
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/version',
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
        .get("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/version")
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
    "version": {
        "major": 21,
        "minor": 0,
        "additional_info": "release"
    },
    "network_version": {
        "chain_name": "TEZOS_MAINNET",
        "distributed_db_version": 2,
        "p2p_version": 1
    },
    "commit_info": {
        "commit_hash": "abcd1234",
        "commit_date": "2025-01-15T00:00:00Z"
    }
}
```

## Response Fields

| Field            | Type   | Description                      |
| ---------------- | ------ | -------------------------------- |
| version          | object | Semantic node version            |
| network\_version | object | Chain name and protocol versions |
| commit\_info     | object | Build commit hash and date       |

## Use Cases

* **Node Identification**: Confirm the Octez build and network behind an endpoint
* **Compatibility**: Check protocol/p2p versions before integrating

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path or body parameter was invalid              |
| 404 / Not found           | Not found     | The block, contract, or resource does not exist   |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
