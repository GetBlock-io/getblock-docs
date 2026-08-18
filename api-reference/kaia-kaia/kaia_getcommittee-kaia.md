---
description: >-
  Example code for the kaia_getCommittee JSON RPC method. Complete guide on how
  to use kaia_getCommittee JSON RPC in GetBlock Web3 documentation.
---

# kaia\_getcommittee

This method returns the addresses of the validators selected as the committee for a given block. The committee is the subset of the council responsible for reaching consensus on that block.

## Parameters

| Parameter | Type   | Required | Description                                                    |
| --------- | ------ | -------- | -------------------------------------------------------------- |
| block     | string | No       | Block number in hex, or "latest". Defaults to the latest block |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "kaia_getCommittee",
    "params": ["0x1b4"],
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
    method: 'kaia_getCommittee',
    params: ["0x1b4"],
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
        'method': 'kaia_getCommittee',
        'params': ["0x1b4"],
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
            "method": "kaia_getCommittee",
            "params": ["0x1b4"],
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
    "result": [
        "0x571e53df607be97431a5bbefca1dffe5aef56f4d",
        "0x5cb1a7dccbd0dc446e3640898ede8820368554c8"
    ]
}
```

## Response Parameters

| Parameter | Type   | Description                                                 |
| --------- | ------ | ----------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                           |
| id        | string | Request identifier matching the request                     |
| result    | array  | Array of validator addresses in the committee for the block |

## Use Cases

* **Consensus Analysis**: Read the committee that finalized a block
* **Validator Participation**: Track how often a validator is on the committee
* **Fault Analysis**: Investigate consensus behavior for a block
* **Explorer Detail**: Show the committee on a block page

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

const committee = await provider.send('kaia_getCommittee', ['0x1b4']);
console.log(committee);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { kaia } from 'viem/chains';

const client = createPublicClient({ chain: kaia, transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/') });

const committee = await client.request({
    method: 'kaia_getCommittee',
    params: ['0x1b4']
});
console.log(committee);
```
{% endcode %}
{% endtab %}
{% endtabs %}
