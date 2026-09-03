# eth\_feeHistory scroll

This method returns a window of historical base fees, gas-used ratios, and requested priority-fee percentiles. It is the basis for building adaptive EIP-1559 fee strategies on GIWA Sepolia.

## Parameters

| Parameter         | Type   | Required | Description                                                     |
| ----------------- | ------ | -------- | --------------------------------------------------------------- |
| blockCount        | string | Yes      | Number of blocks in the history window (hex, 1-1024)            |
| newestBlock       | string | Yes      | Newest block of the range in hex, or "latest"/"pending"         |
| rewardPercentiles | array  | No       | Ascending list of percentiles (0-100) for priority-fee sampling |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_feeHistory",
    "params": ["0x5", "latest", [10, 50, 90]],
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
    method: 'eth_feeHistory',
    params: ['0x5', 'latest', [10, 50, 90]],
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
        'method': 'eth_feeHistory',
        'params': ['0x5', 'latest', [10, 50, 90]],
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
            "method": "eth_feeHistory",
            "params": ["0x5", "latest", [10, 50, 90]],
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
        "oldestBlock": "0x14b8a10",
        "baseFeePerGas": [
            "0x7",
            "0x8",
            "0x7",
            "0x7",
            "0x8",
            "0x7"
        ],
        "gasUsedRatio": [
            0.42,
            0.51,
            0.33,
            0.61,
            0.47
        ],
        "reward": [
            [
                "0x0",
                "0xf4240",
                "0x1e8480"
            ]
        ]
    }
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| id        | string | Request identifier matching the request |
| result    | object | Fee history window (see fields below)   |

### Result Object

| Field         | Type   | Description                                                       |
| ------------- | ------ | ----------------------------------------------------------------- |
| oldestBlock   | string | Lowest block number of the returned range (hex)                   |
| baseFeePerGas | array  | Base fee per gas for each block, plus the next block (hex)        |
| gasUsedRatio  | array  | Ratio of gas used to gas limit per block (float 0-1)              |
| reward        | array  | Per-block priority-fee samples at the requested percentiles (hex) |

## Use Cases

* **Adaptive Fees**: Pick a priority fee from a recent percentile distribution
* **Congestion Signals**: Read gasUsedRatio to detect busy periods
* **Fee Charts**: Render base-fee trends in a wallet or dashboard
* **Retry Pricing**: Bump fees on a stuck transaction using recent percentiles

## Error Handling

| Error Code | Message        | Description                                          |
| ---------- | -------------- | ---------------------------------------------------- |
| -32602     | Invalid params | blockCount out of range or malformed percentile list |
| -32603     | Internal error | The node failed to assemble the fee history          |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const history = await provider.send('eth_feeHistory', ['0x5', 'latest', [10, 50, 90]]);
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

const history = await client.getFeeHistory({ blockCount: 5, rewardPercentiles: [10, 50, 90] });
```
{% endcode %}
{% endtab %}
{% endtabs %}
