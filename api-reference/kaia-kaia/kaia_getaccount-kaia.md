---
description: >-
  Example code for the kaia_getAccount JSON RPC method. Complete guide on how to
  use kaia_getAccount JSON RPC in GetBlock Web3 documentation.
---

# kaia\_getaccount

This method returns the Kaia account object for an address. Kaia distinguishes an Externally Owned Account (accType 1) from a Smart Contract Account (accType 2), and exposes the account's balance in kei, nonce, and account key.

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
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "kaia_getAccount",
    "params": ["0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "latest"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'kaia_getAccount',
    params: ["0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "latest"],
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
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'kaia_getAccount',
        'params': ["0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "latest"],
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
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "kaia_getAccount",
            "params": ["0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "latest"],
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
    "result": {
        "accType": 1,
        "account": {
            "balance": "0x82127e178c5f1c000",
            "humanReadable": false,
            "key": {
                "keyType": 1,
                "key": {}
            },
            "nonce": 3
        }
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                                |
| --------- | ------ | ---------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                          |
| id        | string | Request identifier matching the request                    |
| result    | object | Kaia account object, or null if the account does not exist |

### Result Object

| Field                 | Type    | Description                                                |
| --------------------- | ------- | ---------------------------------------------------------- |
| accType               | integer | Account type: 1 for an EOA, 2 for a Smart Contract Account |
| account.balance       | string  | Account balance in kei, hex-encoded                        |
| account.nonce         | integer | Account nonce                                              |
| account.key           | object  | The account key, with its keyType and key data             |
| account.humanReadable | boolean | Whether the address is a human-readable address (reserved) |

## Use Cases

* **Account Inspection**: Read a Kaia account's type, balance, and key together
* **Contract Detection**: Distinguish an EOA from a Smart Contract Account by accType
* **Key Discovery**: Read the account key attached to an account
* **Existence Checks**: Detect whether an address exists in state

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

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const account = await provider.send('kaia_getAccount', ['0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045', 'latest']);
console.log(account);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { kaia } from 'viem/chains';

const client = createPublicClient({ chain: kaia, transport: http('https://go.getblock.io/<ACCESS-TOKEN>/') });

const account = await client.request({
    method: 'kaia_getAccount',
    params: ['0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045', 'latest']
});
console.log(account);
```
{% endcode %}
{% endtab %}
{% endtabs %}
