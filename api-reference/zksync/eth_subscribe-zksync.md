# eth\_subscribe zksync

Creates a real-time subscription over a WebSocket connection. Available subscription types are `newHeads` (new block headers) and `logs` (matching contract events). Only available over WebSocket transport — use `wss://go.getblock.io/<ACCESS-TOKEN>/`. Note: zkSync does not support the `syncing` subscription type.

## Parameters

| Parameter        | Type   | Required | Description                                         |
| ---------------- | ------ | -------- | --------------------------------------------------- |
| subscriptionType | string | Yes      | One of `"newHeads"`, `"logs"`                       |
| filterObject     | object | No       | For `"logs"`: filter with `address` and/or `topics` |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_subscribe",
    "params": [
        "newHeads"
    ],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_subscribe",
    "params": [
        "newHeads"
    ],
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers: {
        'Content-Type': 'application/json'
    },
    data: data
};

axios(config)
    .then(response => console.log(JSON.stringify(response.data, null, 2)))
    .catch(error => console.log(error));
```
{% endtab %}

{% tab title="Python (Requests)" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_subscribe",
    "params": [
        "newHeads"
    ],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endtab %}

{% tab title="Rust (Reqwest)" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let payload = json!({
        "jsonrpc": "2.0",
        "method": "eth_subscribe",
        "params": [
                "newHeads"
        ],
        "id": "getblock.io"
});

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&payload)
        .send()
        .await?;

    let result: serde_json::Value = response.json().await?;
    println!("{:#?}", result);

    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x9cef478923ff08bf67fde6c64013158d"
}
```
{% endcode %}

## Response Parameters

| Field  | Type   | Description                                                                                            |
| ------ | ------ | ------------------------------------------------------------------------------------------------------ |
| result | string | Subscription ID, used to match incoming `eth_subscription` notifications and to call `eth_unsubscribe` |

## Use Cases

* Real-time wallet activity notifications
* Live DEX trade feeds for trading UIs
* Streaming indexers that react to new L2 blocks instantly

## Error Handling

| Status Code | Error Message     | Cause                                                                    |
| ----------- | ----------------- | ------------------------------------------------------------------------ |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                                      |
| -32602      | Invalid params    | Request parameters are missing or malformed                              |
| -32601      | Method not found  | Method does not exist or is not enabled on this node                     |
| 429         | Too Many Requests | Rate limit exceeded for your plan                                        |
| -32600      | Invalid Request   | `eth_subscribe` was called over HTTP; use a WebSocket connection instead |

## SDK Integration

{% tabs %}
{% tab title="zksync-ethers (WebSocket)" %}
```javascript
import { Provider } from 'zksync-ethers';

const provider = Provider.getDefaultProvider();
// Or with explicit WebSocket: new Provider('wss://go.getblock.io/<ACCESS-TOKEN>/');

provider.on('block', (blockNumber) => {
    console.log('New block:', blockNumber);
});
```
{% endtab %}

{% tab title="Viem (WebSocket)" %}
```javascript
import { createPublicClient, webSocket } from 'viem';
import { zksync } from 'viem/chains';

const client = createPublicClient({
    chain: zksync,
    transport: webSocket('wss://go.getblock.io/<ACCESS-TOKEN>/')
});

const unwatch = client.watchBlockNumber({
    onBlockNumber: (blockNumber) => console.log('New block:', blockNumber)
});
```
{% endtab %}
{% endtabs %}
