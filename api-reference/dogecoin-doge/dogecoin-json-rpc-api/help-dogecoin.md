---
description: >-
  Example code for the help JSON-RPC method. Complete guide on how to use
  help JSON-RPC in GetBlock Web3 documentation.
---

# help - Dogecoin

This method lists all commands the node exposes, or returns the help text for a single command when one is named.

## Parameters

| Parameter | Type   | Required | Description                                                     |
| --------- | ------ | -------- | ----------------------------------------------------------------- |
| command   | string | No       | The command to describe. Omit it to list every available command |

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
  jsonrpc: '2.0', method: 'help', params: ["getblockcount"], id: 'getblock.io'
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

```json
{
    "error": null,
    "id": "getblock.io",
    "result": "getblockcount\n\nReturns the number of blocks in the longest blockchain.\n\nResult:\nn    (numeric) The current block count\n"
}
```

## Response Parameters

| Parameter | Type         | Description                                      |
| --------- | ------------ | ------------------------------------------------ |
| error     | null\|object | Error object when the call fails, otherwise null |
| id        | string       | Request identifier matching the request          |
| result | string | Help text for the named command, or the full command list |

{% hint style="info" %}
Calling `help` with no parameter is the authoritative way to see which methods a given endpoint actually serves. Dogecoin Core is based on an older Bitcoin Core release than most UTXO chains, so methods added to Bitcoin Core later — the PSBT family, `getblockstats`, `testmempoolaccept`, `signrawtransactionwithkey` — are not present. Wallet methods are disabled on shared nodes regardless of whether `help` lists them.
{% endhint %}

## Use Cases

* **Capability Discovery**: List exactly which methods this endpoint serves
* **Version Differences**: Confirm whether a method exists before depending on it
* **Debugging**: Read a method's expected parameters straight from the node

## Error Handling

| Error Code | Message | Description |
| ---------- | ------- | ----------- |
| -32700 | Parse error | Request body is not valid JSON |
| -32600 | Invalid request | The JSON sent is not a valid request object |
| -32603 | Internal error | Node failed to read the requested chain state |
