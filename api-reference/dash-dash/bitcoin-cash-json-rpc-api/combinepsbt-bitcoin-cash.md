---
description: >-
  Example code for the combinepsbt JSON-RPC method. Complete guide on how to use
  combinepsbt JSON-RPC in GetBlock Web3 documentation.
---

# combinepsbt - Bitcoin Cash

This method merges multiple partially signed transactions for the same underlying transaction into a single PSBT, combining their signatures.

## Parameters

| Parameter | Type  | Required | Description                              |
| --------- | ----- | -------- | ---------------------------------------- |
| txs       | array | Yes      | Array of base64-encoded PSBTs to combine |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "combinepsbt",
    "params": [["cHNidP8BAHECAAAAAeJ+A9IdBRqivZ0zbDrAaTz6ks5xWSzuxSGxxIAZ/3ehAQAAAAD/////AqGaAQAAAAAAFgAUQiAC2SehyukB6sZoREzOjdCuYNUps7ELAAAAABepFPW0jREw3D02bR6r9ng6VS0cjgj0hwAAAAAAAAAA", "cHNidP8BAHECAAAAAeJ+A9IdBRqivZ0zbDrAaTz6ks5xWSzuxSGxxIAZ/3ehAQAAAAD/////AqGaAQAAAAAAFgAUQiAC2SehyukB6sZoREzOjdCuYNUps7ELAAAAABepFPW0jREw3D02bR6r9ng6VS0cjgj0hwAAAAAAAAAA"]],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="bitcoinjs-lib" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const { data } = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
  jsonrpc: '2.0', method: 'combinepsbt',
  params: [[psbtA, psbtB]], id: 'getblock.io'
});
console.log(data.result);
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
        'method': 'combinepsbt',
        'params': [["cHNidP8BAHECAAAAAeJ+A9IdBRqivZ0zbDrAaTz6ks5xWSzuxSGxxIAZ/3ehAQAAAAD/////AqGaAQAAAAAAFgAUQiAC2SehyukB6sZoREzOjdCuYNUps7ELAAAAABepFPW0jREw3D02bR6r9ng6VS0cjgj0hwAAAAAAAAAA", "cHNidP8BAHECAAAAAeJ+A9IdBRqivZ0zbDrAaTz6ks5xWSzuxSGxxIAZ/3ehAQAAAAD/////AqGaAQAAAAAAFgAUQiAC2SehyukB6sZoREzOjdCuYNUps7ELAAAAABepFPW0jREw3D02bR6r9ng6VS0cjgj0hwAAAAAAAAAA"]],
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
            "method": "combinepsbt",
            "params": [["cHNidP8BAHECAAAAAeJ+A9IdBRqivZ0zbDrAaTz6ks5xWSzuxSGxxIAZ/3ehAQAAAAD/////AqGaAQAAAAAAFgAUQiAC2SehyukB6sZoREzOjdCuYNUps7ELAAAAABepFPW0jREw3D02bR6r9ng6VS0cjgj0hwAAAAAAAAAA", "cHNidP8BAHECAAAAAeJ+A9IdBRqivZ0zbDrAaTz6ks5xWSzuxSGxxIAZ/3ehAQAAAAD/////AqGaAQAAAAAAFgAUQiAC2SehyukB6sZoREzOjdCuYNUps7ELAAAAABepFPW0jREw3D02bR6r9ng6VS0cjgj0hwAAAAAAAAAA"]],
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
    "error": null,
    "id": "getblock.io",
    "result": "cHNidP8BAHECAAAAAeJ+A9IdBRqivZ0zbDrAaTz6ks5xWSzuxSGxxIAZ/3ehAQAAAAD/////AqGaAQAAAAAAFgAUQiAC2SehyukB6sZoREzOjdCuYNUps7ELAAAAABepFPW0jREw3D02bR6r9ng6VS0cjgj0hwAAAAAAAAAA"
}
```

## Response Parameters

| Parameter | Type         | Description                                      |
| --------- | ------------ | ------------------------------------------------ |
| error     | null\|object | Error object when the call fails, otherwise null |
| id        | string       | Request identifier matching the request          |
| result    | string       | Base64-encoded PSBT combining all inputs         |

## Use Cases

* **Multisig Aggregation**: Merge signatures from separate co-signers into one PSBT
* **Distributed Signing**: Collect partial signatures produced independently
* **Threshold Wallets**: Assemble the required signatures for an m-of-n spend
* **Workflow Completion**: Consolidate a PSBT before finalizing it

## Error Handling

| Error Code | Message           | Description                                  |
| ---------- | ----------------- | -------------------------------------------- |
| -22        | Decode failed     | The supplied data could not be decoded       |
| -8         | Invalid parameter | A required parameter is missing or malformed |
| -32603     | Internal error    | Node failed to process the request           |
