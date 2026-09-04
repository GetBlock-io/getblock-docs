# validators - Cronos

Returns the paginated validator set active at a given height, with each validator's consensus address, public key, and voting power.

## Parameters

| Parameter | Type   | Required | Description                           |
| --------- | ------ | -------- | ------------------------------------- |
| height    | string | Optional | Block height; omit for the latest set |
| page      | string | Optional | 1-based page number                   |
| per\_page | string | Optional | Results per page (max 100)            |

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
    "params": {
    "height": "12345678",
    "page": "1",
    "per_page": "100"
}
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    id: 'getblock.io',
    method: 'validators',
    params: {
    "height": "12345678",
    "page": "1",
    "per_page": "100"
}
}, { headers: { 'Content-Type': 'application/json' } });

console.log(response.data.result);
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
        'id': 'getblock.io',
        'method': 'validators',
        'params': {
    "height": "12345678",
    "page": "1",
    "per_page": "100"
}
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
use serde_json::{json, Value};

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    let response = client
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/")
        .json(&json!({
            "jsonrpc": "2.0",
            "id": "getblock.io",
            "method": "validators",
            "params": {
    "height": "12345678",
    "page": "1",
    "per_page": "100"
}
        }))
        .send().await?
        .json::<Value>().await?;
    println!("{}", response["result"]);
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
        "block_height": "12345678",
        "validators": [
            {
                "address": "F00D...",
                "pub_key": {
                    "type": "tendermint/PubKeyEd25519",
                    "value": "b64=="
                },
                "voting_power": "1000000",
                "proposer_priority": "0"
            }
        ],
        "count": "1",
        "total": "30"
    }
}
```

## Response Fields

| Field         | Type   | Description                                                 |
| ------------- | ------ | ----------------------------------------------------------- |
| validators    | array  | Consensus validators with address, pubkey, and voting power |
| total         | string | Total number of validators in the set                       |
| block\_height | string | Height the set applies to                                   |

## Use Cases

* **Consensus Monitoring**: Track the active validator set and voting power
* **Proposer Analysis**: Read proposer\_priority across validators
* **Staking Dashboards**: Pair consensus addresses with operator addresses

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| -32603 / Internal error   | Internal error | Height is out of range                            |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
