# eth\_getBlockTransactionCountByNumber - Moonbeam

This method returns the number of transactions in a block identified by its number or tag, as a hex-encoded integer.

## Parameters

| Parameter      | Type   | Required | Description                                             |
| -------------- | ------ | -------- | ------------------------------------------------------- |
| blockParameter | string | Yes      | Block number in hex, or "latest", "earliest", "pending" |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBlockTransactionCountByNumber",
    "params": ["latest"],
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
    method: 'eth_getBlockTransactionCountByNumber',
    params: ['latest'],
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
        'method': 'eth_getBlockTransactionCountByNumber',
        'params': ['latest'],
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
            "method": "eth_getBlockTransactionCountByNumber",
            "params": ["latest"],
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
    "result": "0x2a"
}
```

## Response Parameters

| Parameter | Type   | Description                                |
| --------- | ------ | ------------------------------------------ |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")          |
| id        | string | Request identifier matching the request    |
| result    | string | Hex-encoded transaction count in the block |

## Use Cases

* **Block Sizing**: Read a block's transaction count by height or tag
* **Index Iteration**: Bound a loop over transaction indices by block number
* **Throughput Metrics**: Sample transaction counts across a range of heights
* **Live Monitoring**: Track fullness of the "latest" block

## Error Handling

| Error Code | Message        | Description                                       |
| ---------- | -------------- | ------------------------------------------------- |
| -32602     | Invalid params | Malformed block number or tag                     |
| -32603     | Internal error | The node failed to count the block's transactions |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const block = await provider.getBlock('latest');
console.log(block.transactions.length);
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

const count = await client.getBlockTransactionCount({ blockTag: 'latest' });
```
{% endcode %}
{% endtab %}
{% endtabs %}
