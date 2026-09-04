# eth\_newFilter - Moonbeam

This method creates a log filter matching address and topic criteria and returns a filter ID. New matching logs are then retrieved with `eth_getFilterChanges`.

## Parameters

| Parameter    | Type   | Required | Description            |
| ------------ | ------ | -------- | ---------------------- |
| filterObject | object | Yes      | Filter criteria object |

### Filter Object

| Field     | Type            | Required | Description                              |
| --------- | --------------- | -------- | ---------------------------------------- |
| fromBlock | string          | No       | Start block in hex, or "latest"          |
| toBlock   | string          | No       | End block in hex, or "latest"            |
| address   | string \| array | No       | Contract address(es) to match            |
| topics    | array           | No       | Ordered topic filters (null matches any) |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_newFilter",
    "params": [{ "fromBlock": "0xb71b00", "toBlock": "latest", "address": "0xAcc15dC74880C9944775448304B263D191c6077F", "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"] }],
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
    method: 'eth_newFilter',
    params: [{ fromBlock: '0xb71b00', toBlock: 'latest', address: '0xAcc15dC74880C9944775448304B263D191c6077F', topics: ['0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef'] }],
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
        'method': 'eth_newFilter',
        'params': [{ 'fromBlock': '0xb71b00', 'toBlock': 'latest', 'address': '0xAcc15dC74880C9944775448304B263D191c6077F', 'topics': ['0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef'] }],
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
            "method": "eth_newFilter",
            "params": [{ "fromBlock": "0xb71b00", "toBlock": "latest", "address": "0xAcc15dC74880C9944775448304B263D191c6077F", "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"] }],
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
    "result": "0x1a4b0d21f8e0c3b6d2a9f7e4c1b8a5d2"
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| id        | string | Request identifier matching the request |
| result    | string | Hex-encoded filter ID                   |

## Use Cases

* **Event Watching**: Track ERC-20 Transfer events for a token contract
* **Polling Pipelines**: Create a filter, then poll changes on an interval
* **Topic Filtering**: Match specific event signatures by topic0
* **Multi-Contract Watch**: Watch several addresses with one filter

## Error Handling

| Error Code | Message        | Description                            |
| ---------- | -------------- | -------------------------------------- |
| -32602     | Invalid params | Malformed filter object or topic list  |
| -32603     | Internal error | The node failed to register the filter |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

// ethers v6 uses provider.on(filter, cb) rather than raw filter IDs
provider.on({ address: '0xAcc15dC74880C9944775448304B263D191c6077F', topics: ['0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef'] }, (log) => console.log(log));
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

const filter = await client.createEventFilter({ address: '0xAcc15dC74880C9944775448304B263D191c6077F' });
```
{% endcode %}
{% endtab %}
{% endtabs %}
