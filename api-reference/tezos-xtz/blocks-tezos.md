---
description: >-
  Example code for the /chains/main/blocks REST method. Complete guide on how to
  use the /chains/main/blocks REST method in GetBlock Web3 documentation
---

# blocks - Tezos

Returns a list of the most recent block hashes on the main chain, most recent first. Supports length and head query parameters to page back through history.

## Endpoint

```http
GET /chains/main/blocks
```

## Query Parameters

| Parameter | Type   | Description                |
| --------- | ------ | -------------------------- |
| length    | string | Number of blocks to return |
| head      | string | Block hash to start from   |

## Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
export TEZOS_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${TEZOS_REST}chains/main/blocks?length=2"
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks', {
    params: { length: 2 },
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks',
    headers={'Content-Type': 'application/json'},
    params={'length': 2}
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
        .get("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks")
        .query(&[("length", "2")])
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
    [
        "BMblockhash0head...",
        "BMblockhash1..."
    ]
]
```

## Response Fields

| Field  | Type  | Description                                        |
| ------ | ----- | -------------------------------------------------- |
| (root) | array | Array of arrays of block hashes, most recent first |

## Use Cases

* **Chain Head**: Fetch the latest block hashes
* **History Paging**: Walk backwards through recent blocks

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path or body parameter was invalid              |
| 404 / Not found           | Not found     | The block, contract, or resource does not exist   |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
