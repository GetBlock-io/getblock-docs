# kaia\_getcouncil

This method returns the addresses of all validators in the governance council at a given block. The council is the full set of validators eligible to participate in consensus.

## Parameters

| Parameter | Type   | Required | Description                                                    |
| --------- | ------ | -------- | -------------------------------------------------------------- |
| block     | string | No       | Block number in hex, or "latest". Defaults to the latest block |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "kaia_getCouncil",
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

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'kaia_getCouncil',
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
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'kaia_getCouncil',
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
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "kaia_getCouncil",
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
        "0x5cb1a7dccbd0dc446e3640898ede8820368554c8",
        "0x99fb17d324fa0e07f23b49d09028ac0919414db6"
    ]
}
```

## Response Parameters

| Parameter | Type   | Description                                              |
| --------- | ------ | -------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                        |
| id        | string | Request identifier matching the request                  |
| result    | array  | Array of validator addresses in the council at the block |

## Use Cases

* **Validator Sets**: Read the full council at a given height
* **Governance Tracking**: Detect changes to the validator set over time
* **Staking Tools**: Enumerate validators available for delegation
* **Network Analytics**: Measure council size and composition

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

const council = await provider.send('kaia_getCouncil', ['0x1b4']);
console.log(council);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { kaia } from 'viem/chains';

const client = createPublicClient({ chain: kaia, transport: http('https://go.getblock.io/<ACCESS-TOKEN>/') });

const council = await client.request({
    method: 'kaia_getCouncil',
    params: ['0x1b4']
});
console.log(council);
```
{% endcode %}
{% endtab %}
{% endtabs %}
