# eth\_unsubscribe - Moonbeam

This method cancels an active WebSocket subscription by its ID and stops further notifications for that subscription.

{% hint style="info" %}
This method works only over a WebSocket connection. Use the `wss://` form of the endpoint (`wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`) rather than HTTPS.
{% endhint %}

## Parameters

| Parameter      | Type   | Required | Description                                |
| -------------- | ------ | -------- | ------------------------------------------ |
| subscriptionId | string | Yes      | Subscription ID returned by eth\_subscribe |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_unsubscribe",
    "params": ["0x9cef478923ff08bf67fde6c64013158d"],
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
    method: 'eth_unsubscribe',
    params: ['0x9cef478923ff08bf67fde6c64013158d'],
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
        'method': 'eth_unsubscribe',
        'params': ['0x9cef478923ff08bf67fde6c64013158d'],
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
            "method": "eth_unsubscribe",
            "params": ["0x9cef478923ff08bf67fde6c64013158d"],
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

| Parameter | Type    | Description                                        |
| --------- | ------- | -------------------------------------------------- |
| jsonrpc   | string  | JSON-RPC protocol version ("2.0")                  |
| id        | string  | Request identifier matching the request            |
| result    | boolean | true if the subscription existed and was cancelled |

## Use Cases

* **Stream Teardown**: Stop a subscription when a consumer disconnects
* **Resource Cleanup**: Release server-side subscription state
* **Subscription Rotation**: Cancel and re-create subscriptions with new filters
* **Graceful Shutdown**: Close subscriptions cleanly on service stop

## Error Handling

| Error Code | Message          | Description                                            |
| ---------- | ---------------- | ------------------------------------------------------ |
| -32602     | Invalid params   | Malformed or unknown subscription ID                   |
| -32601     | Method not found | The endpoint was called over HTTP instead of WebSocket |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

// ethers v6: provider.destroy() closes the socket and all subscriptions
await provider.destroy();
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

// viem: call the unwatch function returned by watchBlocks/watchEvent
unwatch();
```
{% endcode %}
{% endtab %}
{% endtabs %}
