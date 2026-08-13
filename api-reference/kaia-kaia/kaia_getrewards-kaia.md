---
description: >-
  Example code for the kaia_getRewards JSON RPC method. Complete guide on how to
  use kaia_getRewards JSON RPC in GetBlock Web3 documentation.
---

# kaia\_getrewards

This method returns the block reward distribution for a given block, including the newly minted KAIA, transaction fees, and how the reward was split between the proposer, stakers, and the ecosystem funds. All amounts are in kei.

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
    "method": "kaia_getRewards",
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
    method: 'kaia_getRewards',
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
        'method': 'kaia_getRewards',
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
            "method": "kaia_getRewards",
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
    "result": {
        "minted": "0x878678326eac900000",
        "totalFee": "0x1bc16d674ec80000",
        "burntFee": "0xde0b6b3a7640000",
        "proposer": "0x43c30 e0000000000",
        "stakers": "0x0",
        "kif": "0x2b5e3af16b1880000",
        "kef": "0x2b5e3af16b1880000",
        "rewards": {
            "0x571e53df607be97431a5bbefca1dffe5aef56f4d": "0x43c30e0000000000"
        }
    }
}
```

## Response Parameters

| Parameter | Type   | Description                              |
| --------- | ------ | ---------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")        |
| id        | string | Request identifier matching the request  |
| result    | object | Reward distribution object for the block |

### Result Object

| Field    | Type   | Description                                         |
| -------- | ------ | --------------------------------------------------- |
| minted   | string | Newly minted KAIA for the block, in kei             |
| totalFee | string | Total transaction fees for the block, in kei        |
| burntFee | string | Portion of fees burned, in kei                      |
| proposer | string | Reward paid to the block proposer, in kei           |
| stakers  | string | Reward allocated to stakers, in kei                 |
| kif      | string | Amount sent to the Kaia Infrastructure Fund, in kei |
| kef      | string | Amount sent to the Kaia Ecosystem Fund, in kei      |
| rewards  | object | Per-address reward amounts, in kei                  |

## Use Cases

* **Reward Accounting**: Read how a block's reward was distributed
* **Staking Yields**: Compute staking rewards from block data
* **Tokenomics Analysis**: Track minted supply and burned fees over time
* **Fund Monitoring**: Follow allocations to the infrastructure and ecosystem funds

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

const rewards = await provider.send('kaia_getRewards', ['0x1b4']);
console.log(rewards);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { kaia } from 'viem/chains';

const client = createPublicClient({ chain: kaia, transport: http('https://go.getblock.io/<ACCESS-TOKEN>/') });

const rewards = await client.request({
    method: 'kaia_getRewards',
    params: ['0x1b4']
});
console.log(rewards);
```
{% endcode %}
{% endtab %}
{% endtabs %}
