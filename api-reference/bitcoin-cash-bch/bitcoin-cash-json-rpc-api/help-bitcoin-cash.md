---
description: >-
  Example code for the help JSON-RPC method. Complete guide on how to use the
  help JSON-RPC method in the GetBlock Web3 documentation.
---

# help - Bitcoin Cash

This method lists all available RPC commands, or returns detailed help for a single command when one is named.

## Parameters

| Parameter | Type   | Required | Description                                                    |
| --------- | ------ | -------- | -------------------------------------------------------------- |
| command   | string | No       | The command to get help for. Default is a list of all commands |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "help",
    "params": ["getblockcount"],
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
  jsonrpc: '2.0', method: 'help',
  params: ['getblockcount'], id: 'getblock.io'
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
        'method': 'help',
        'params': ["getblockcount"],
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
            "method": "help",
            "params": ["getblockcount"],
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
    "result": "getblockcount\n\nReturns the number of blocks in the longest blockchain.\n\nResult:\nn    (numeric) The current block count\n\nExamples:\n> bitcoin-cli getblockcount\n> curl --user myusername --data-binary '{\"jsonrpc\": \"1.0\", \"id\":\"curltest\", \"method\": \"getblockcount\", \"params\": [] }' -H 'content-type: text/plain;' http://127.0.0.1:8332/\n",
    "error": null,
    "id": "getblock.io"
}
```
{% endcode %}

## Response Parameters

| Parameter | Type         | Description                                          |
| --------- | ------------ | ---------------------------------------------------- |
| error     | null\|object | Error object when the call fails, otherwise null     |
| id        | string       | Request identifier matching the request              |
| result    | string       | Help text for the command, or a list of all commands |

## Use Cases

* **API Discovery**: List available commands when exploring the interface
* **Parameter Reference**: Read argument details for a specific command
* **Tooling**: Generate command inventories programmatically
* **Debugging**: Confirm a command exists on the connected node build

## Error Handling

| Error Code | Message         | Description                                   |
| ---------- | --------------- | --------------------------------------------- |
| -32700     | Parse error     | Request body is not valid JSON                |
| -32600     | Invalid request | The JSON sent is not a valid request object   |
| -32603     | Internal error  | Node failed to read the requested chain state |
