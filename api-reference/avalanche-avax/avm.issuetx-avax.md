---
description: >-
  Example code for the avm.issueTx JSON-RPC method. Complete guide on how to use
  avm.issueTx JSON-RPC in GetBlock Web3 documentation.
---

# avm.issueTx - AVAX

Broadcasts a signed X-Chain transaction to the network. The transaction must be constructed and signed offline (typically via AvalancheJS) and submitted as an encoded blob.

## Parameters

| Parameter | Type   | Required | Description                              |
| --------- | ------ | -------- | ---------------------------------------- |
| tx        | string | Yes      | Signed transaction blob                  |
| encoding  | string | No       | Encoding — `"hex"` (default) or `"cb58"` |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/X' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "avm.issueTx",
    "params": {
        "tx": "0x0000000000000003000000000000000000000000000000000000000000000000000000000000000000000000...",
        "encoding": "hex"
    },
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "avm.issueTx",
    "params": {
        "tx": "0x0000000000000003000000000000000000000000000000000000000000000000000000000000000000000000...",
        "encoding": "hex"
    },
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/X',
    headers: {
        'Content-Type': 'application/json'
    },
    data: data
};

axios(config)
    .then(response => console.log(JSON.stringify(response.data, null, 2)))
    .catch(error => console.log(error));
```
{% endtab %}

{% tab title="Python (Requests)" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/X"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "avm.issueTx",
    "params": {
        "tx": "0x0000000000000003000000000000000000000000000000000000000000000000000000000000000000000000...",
        "encoding": "hex"
    },
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endtab %}

{% tab title="Rust (Reqwest)" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let payload = json!({
        "jsonrpc": "2.0",
        "method": "avm.issueTx",
        "params": {
                "tx": "0x0000000000000003000000000000000000000000000000000000000000000000000000000000000000000000...",
                "encoding": "hex"
        },
        "id": "getblock.io"
});

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/X")
        .header("Content-Type", "application/json")
        .json(&payload)
        .send()
        .await?;

    let result: serde_json::Value = response.json().await?;
    println!("{:#?}", result);

    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Response Example

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "txID": "2kxwWpHvZPhMsJcSTmM7a3Da7sExB8pPyF7t4cr2NSwnYqNHni"
    }
}
```

## Response Parameters

| Field       | Type   | Description                                               |
| ----------- | ------ | --------------------------------------------------------- |
| result.txID | string | Transaction ID — use `avm.getTxStatus` to track inclusion |

## Use Cases

* Broadcasting X-Chain asset transfers
* Creating new native assets on Avalanche
* Atomic cross-chain operations from X-Chain

## Error Handling

| Status Code | Error Message       | Cause                                                    |
| ----------- | ------------------- | -------------------------------------------------------- |
| 404         | Not Found           | Missing or invalid `<ACCESS-TOKEN>`                      |
| -32602      | Invalid params      | Request parameters are missing or malformed              |
| 429         | Too Many Requests   | Rate limit exceeded for your plan                        |
| -32000      | Invalid transaction | Signature invalid, malformed body, or insufficient funds |

## SDK Integration

{% tabs %}
{% tab title="AvalancheJS / Axios" %}
```javascript
// AvalancheJS provides typed clients for P-Chain and X-Chain.
// X-Chain operations are exposed through `avm` in AvalancheJS.
// For raw JSON-RPC access, use a generic HTTP client as in the Request Example.
import axios from 'axios';

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/X', {
    jsonrpc: '2.0',
    id: 'getblock.io',
    method: 'avm.issueTx',
    params: {"tx": "0x0000000000000003000000000000000000000000000000000000000000000000000000000000000000000000...", "encoding": "hex"}
}});
console.log(response.data);
```
{% endtab %}

{% tab title="Python" %}
```python
# AvalancheJS is JavaScript-only. For Python, use a generic JSON-RPC client.
import requests
import json

url = 'https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/X'
payload = {
    'jsonrpc': '2.0',
    'id': 'getblock.io',
    'method': 'avm.issueTx',
    'params': {"tx": "0x0000000000000003000000000000000000000000000000000000000000000000000000000000000000000000...", "encoding": "hex"}
}}
response = requests.post(url, json=payload)
print(response.json())
```
{% endtab %}
{% endtabs %}
