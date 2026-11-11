---
description: >-
  Example code for the kaia_isContractAccount JSON RPC method. Complete guide on
  how to use kaia_isContractAccount JSON RPC in GetBlock Web3 documentation.
---

# kaia\_iscontractaccount

This method returns true if the address is a Smart Contract Account and false if it is an Externally Owned Account. It reflects Kaia's explicit account-type model rather than inferring contract status from code length.

## Parameters

| Parameter | Type   | Required | Description                                             |
| --------- | ------ | -------- | ------------------------------------------------------- |
| address   | string | Yes      | 20-byte account address                                 |
| block     | string | Yes      | Block number in hex, or "latest", "earliest", "pending" |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "kaia_isContractAccount",
    "params": ["0x19aac5f612f524b754ca7e7c41cbfa2e981a4432", "latest"],
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
    method: 'kaia_isContractAccount',
    params: ["0x19aac5f612f524b754ca7e7c41cbfa2e981a4432", "latest"],
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
        'method': 'kaia_isContractAccount',
        'params': ["0x19aac5f612f524b754ca7e7c41cbfa2e981a4432", "latest"],
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
            "method": "kaia_isContractAccount",
            "params": ["0x19aac5f612f524b754ca7e7c41cbfa2e981a4432", "latest"],
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
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": true
}
```

## Response Parameters

| Parameter | Type    | Description                                         |
| --------- | ------- | --------------------------------------------------- |
| jsonrpc   | string  | JSON-RPC protocol version ("2.0")                   |
| id        | string  | Request identifier matching the request             |
| result    | boolean | true for a Smart Contract Account, false for an EOA |

## Use Cases

* **Contract Detection**: Confirm an address is a contract before calling it
* **Wallet UX**: Warn users before sending assets to a contract
* **Indexing**: Classify addresses by account type
* **Validation**: Gate logic on whether a target is a contract

## Error Handling

| Error Code | Message        | Description                                                |
| ---------- | -------------- | ---------------------------------------------------------- |
| -32602     | Invalid params | A parameter is missing or has the wrong type or format     |
| -32000     | Not found      | The account or block does not exist in the requested state |
| -32603     | Internal error | The node failed to process the request                     |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const isContract = await provider.send('kaia_isContractAccount', ['0x19aac5f612f524b754ca7e7c41cbfa2e981a4432', 'latest']);
console.log(isContract);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { kaia } from 'viem/chains';

const client = createPublicClient({ chain: kaia, transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/') });

const isContract = await client.request({
    method: 'kaia_isContractAccount',
    params: ['0x19aac5f612f524b754ca7e7c41cbfa2e981a4432', 'latest']
});
console.log(isContract);
```
{% endcode %}
{% endtab %}
{% endtabs %}
