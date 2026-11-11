---
description: >-
  Example code for the validateaddress JSON-RPC method. Complete guide on how to
  use the validateaddress JSON-RPC method in the GetBlock Web3 documentation.
---

# validateaddress - Bitcoin Cash

This method returns information about whether a given Bitcoin Cash address is valid, along with details derived from the address when it is.

## Parameters

| Parameter | Type   | Required | Description                          |
| --------- | ------ | -------- | ------------------------------------ |
| address   | string | Yes      | The Bitcoin Cash address to validate |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "validateaddress",
    "params": ["bitcoincash:qpm2qsznhks23z7629mms6s4cwef74vcwvy22gdx6a"],
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
  jsonrpc: '2.0', method: 'validateaddress',
  params: ['bitcoincash:qpm2qsznhks23z7629mms6s4cwef74vcwvy22gdx6a'], id: 'getblock.io'
});
console.log(data.result.isvalid);
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
        'method': 'validateaddress',
        'params': ["bitcoincash:qpm2qsznhks23z7629mms6s4cwef74vcwvy22gdx6a"],
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
            "method": "validateaddress",
            "params": ["bitcoincash:qpm2qsznhks23z7629mms6s4cwef74vcwvy22gdx6a"],
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
    "result": {
        "isvalid": true,
        "address": "bitcoincash:qpm2qsznhks23z7629mms6s4cwef74vcwvy22gdx6a",
        "scriptPubKey": "76a91476a04053bda0a88bda5177b86a15c3b29f55987388ac",
        "isscript": false
    }
}
```

## Response Parameters

| Parameter | Type         | Description                                            |
| --------- | ------------ | ------------------------------------------------------ |
| error     | null\|object | Error object when the call fails, otherwise null       |
| id        | string       | Request identifier matching the request                |
| result    | object       | Object describing address validity and derived details |

### Result Object

| Field        | Type    | Description                             |
| ------------ | ------- | --------------------------------------- |
| isvalid      | boolean | Whether the address is valid            |
| address      | string  | The validated address                   |
| scriptPubKey | string  | The output script the address maps to   |
| isscript     | boolean | Whether the address is a script address |

## Use Cases

* **Input Validation**: Reject malformed addresses before building a payment
* **Wallet UX**: Give immediate feedback on address entry
* **Address Typing**: Distinguish P2PKH from P2SH addresses
* **Payment Safety**: Confirm a destination address is well-formed before sending

## Error Handling

| Error Code | Message           | Description                                  |
| ---------- | ----------------- | -------------------------------------------- |
| -22        | Decode failed     | The supplied data could not be decoded       |
| -8         | Invalid parameter | A required parameter is missing or malformed |
| -32603     | Internal error    | Node failed to process the request           |
