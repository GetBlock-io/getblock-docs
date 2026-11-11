---
description: >-
  Example code for the kaia_getAccountKey JSON RPC method. Complete guide on how
  to use kaia_getAccountKey JSON RPC in GetBlock Web3 documentation.
---

# kaia\_getaccountkey

This method returns the account key of an Externally Owned Account. Kaia decouples the key from the address, so an account can use a public key, a weighted multisig key, or a role-based key. It returns null for a Legacy account or a Smart Contract Account.

## Parameters

| Parameter | Type   | Required | Description                                             |
| --------- | ------ | -------- | ------------------------------------------------------- |
| address   | string | Yes      | 20-byte EOA address                                     |
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
    "method": "kaia_getAccountKey",
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

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'kaia_getAccountKey',
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'kaia_getAccountKey',
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
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "kaia_getAccountKey",
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
        "keyType": 2,
        "key": {
            "x": "0xc10a08f7...b3a0f6b5",
            "y": "0x7a0d5f2e...9c14e8d1"
        }
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                                                   |
| --------- | ------ | ----------------------------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                                             |
| id        | string | Request identifier matching the request                                       |
| result    | object | Account key object with its keyType, or null for a Legacy or contract account |

### Result Object

| Field   | Type    | Description                                                           |
| ------- | ------- | --------------------------------------------------------------------- |
| keyType | integer | Key type: 1 Legacy, 2 Public, 3 Fail, 4 WeightedMultiSig, 5 RoleBased |
| key     | object  | The key payload; shape depends on keyType                             |

## Use Cases

* **Key Management**: Read the key currently controlling an account
* **Multisig Wallets**: Inspect a weighted multisig account key
* **Role Separation**: Read a role-based key that splits signing roles
* **Security Audits**: Verify which key type secures an account

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

const key = await provider.send('kaia_getAccountKey', ['0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045', 'latest']);
console.log(key);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { kaia } from 'viem/chains';

const client = createPublicClient({ chain: kaia, transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/') });

const key = await client.request({
    method: 'kaia_getAccountKey',
    params: ['0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045', 'latest']
});
console.log(key);
```
{% endcode %}
{% endtab %}
{% endtabs %}
