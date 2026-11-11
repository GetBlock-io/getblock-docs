---
description: >-
  Example code for the z_getsubtreesbyindex JSON-RPC method. Complete guide on
  how to use the z_getsubtreesbyindex JSON-RPC method in GetBlock.io Web3
  documentation.
---

# z\_getsubtreesbyindex - Zcash

This method returns note commitment subtree roots by index. Introduced with the Ironwood note commitment tree extension (post-NU6.3), subtree roots enable efficient light-client synchronization: wallets can download and verify chunks of the shielded pool without downloading every commitment. Query the Sapling or Orchard pool independently.

## Parameters

| Parameter    | Type    | Required | Description                                                 |
| ------------ | ------- | -------- | ----------------------------------------------------------- |
| `pool`       | string  | Yes      | Shielded pool name: `sapling` or `orchard`                  |
| `start_index` | integer | Yes      | Zero-based index of the first subtree to return             |
| `limit`      | integer | No       | Maximum number of subtrees to return. Default all available |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "z_getsubtreesbyindex",
    "params": [
        "orchard",
        0,
        10
    ],
    "id": "getblock.io"
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
    method: 'z_getsubtreesbyindex',
    params: [
        "orchard",
        0,
        10
    ],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

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
        'method': 'z_getsubtreesbyindex',
        'params': [
        "orchard",
        0,
        10
    ],
        'id': 'getblock.io'
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
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "z_getsubtreesbyindex",
            "params": [
        "orchard",
        0,
        10
    ],
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

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "result": {
        "pool": "orchard",
        "start_index": 0,
        "subtrees": [
            {
                "root": "d4e323b3ae0cabfb6be4087fec8c66d9a9bbfc354bf1d9588b6620448182063b",
                "end_height": 1707429
            },
            {
                "root": "8c47d0ca43f323ac573ee57c90af4ced484682827248ca5f3eead95eb6415a14",
                "end_height": 1708132
            }
        ]
    },
    "id": "getblock.io"
}
```
{% endcode %}

## Response Parameters

| Parameter               | Type            | Description                                      |
| ----------------------- | --------------- | ------------------------------------------------ |
| `pool`                  | string          | Pool name (`sapling` or `orchard`)               |
| `start_index`            | integer         | Starting subtree index                           |
| `subtrees`              | array of object | List of subtree records                          |
| `subtrees[].root`       | string          | Hex-encoded subtree root                         |
| `subtrees[].end_height` | integer         | Block height at which this subtree was completed |

## Use Cases

* **Light Client Fast Sync**: Wallets download subtree roots to skip commitment-by-commitment iteration during initial sync
* **Ironwood NCT Verification**: Independently verify note commitment tree state via subtree Merkle roots
* **Shielded Pool Growth Analytics**: Track subtree completion cadence to visualize shielded-pool activity growth
* **Feature Detection**: Post-Ironwood extension — availability confirms the endpoint runs a recent-enough Zebra version

## Error Handling

| Error Code | Message        | Description                                  |
| ---------- | -------------- | -------------------------------------------- |
| -8         | Invalid pool   | Pool parameter is not `sapling` or `orchard` |
| -32602     | Invalid params | start_index or limit is out of range          |
| -32603     | Internal error | Node failed to retrieve subtree data         |
