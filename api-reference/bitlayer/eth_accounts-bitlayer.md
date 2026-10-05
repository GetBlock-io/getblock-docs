---
description: >-
  Example code for the eth_accounts JSON-RPC method. Complete guide on how to
  use eth_accounts JSON-RPC in GetBlock Web3 documentation.
---

# eth\_accounts - Bitlayer

This method returns the list of addresses owned by the connected client. On public GetBlock endpoints no keys are held, so it returns an empty array.

## Parameters

{% hint style="info" %}
This method does not require any parameters. Send the request with an empty `params` array.
{% endhint %}

## Request

Use one of these request examples:

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_accounts",
    "params": [],
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
    method: 'eth_accounts',
    params: [],
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
        'method': 'eth_accounts',
        'params': [],
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
            "method": "eth_accounts",
            "params": [],
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

A successful response is:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": []
}
```

## Response parameters

| Parameter | Type   | Description                                             |
| --------- | ------ | ------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                       |
| id        | string | Request identifier matching the request                 |
| result    | array  | Array of client-managed addresses (empty on public RPC) |

## Use cases

* **Key custody check**: Confirm an endpoint holds no signing keys before assuming local signing
* **Tooling compatibility**: Satisfy libraries that probe for node-managed accounts
* **Environment detection**: Distinguish a managed node from a local dev node with unlocked keys
* **Signing path selection**: Fall back to client-side signing when the array is empty

## Error handling

| Error Code | Message          | Description                                   |
| ---------- | ---------------- | --------------------------------------------- |
| -32603     | Internal error   | The node failed to enumerate managed accounts |
| -32601     | Method not found | The method is disabled on the client build    |

## Web3 integration

Use either library:

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const accounts = await provider.send('eth_accounts', []);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';

const client = createPublicClient({
  transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/')
});

const accounts = await client.request({ method: 'eth_accounts' });
```
{% endcode %}
{% endtab %}
{% endtabs %}
