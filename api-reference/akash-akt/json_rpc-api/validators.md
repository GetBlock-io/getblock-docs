---
description: >-
  Example code for the validators JSON-RPC method. Complete guide on how to
  use validators JSON-RPC in GetBlock Web3 documentation.
---

# validators - Akash

Returns the paginated validator set at a height, with each validator's address, public key, and voting power.

## Parameters

| Parameter | Type   | Required | Description      |
| --------- | ------ | -------- | ---------------- |
| height    | string | Optional | Block height     |
| page      | string | Optional | Page number      |
| per\_page | string | Optional | Results per page |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "validators",
    "params": {"height": "19500000", "page": "1", "per_page": "100"}
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'validators', params: {"height": "19500000", "page": "1", "per_page": "100"} }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'validators', 'params': {"height": "19500000", "page": "1", "per_page": "100"}})
print(response.json())
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"validators","params":{"height": "19500000", "page": "1", "per_page": "100"}})).send().await?.json::<Value>().await?;
    println!("{}", res["result"]);
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
    "result": {
        "block_height": "19500000",
        "validators": [
            {
                "address": "F00D...",
                "voting_power": "5000000"
            }
        ],
        "total": "100"
    }
}
```

## Response Fields

| Field      | Type   | Description                                   |
| ---------- | ------ | --------------------------------------------- |
| validators | array  | Validators with address, pubkey, voting power |
| total      | string | Total validators                              |

## Use Cases

* **Consensus Monitoring**: Track the validator set
* **Staking**: Pair with operator addresses
* **Analytics**: Study voting power

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| -32603 / Internal error   | Internal error | Height out of range                               |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
