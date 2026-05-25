# get\_coinbase\_tx\_sum monero

This method returns the sum of coinbase transactions (block rewards) over a specified range of blocks. Useful for emission tracking and economic analysis.

## Parameters

| Parameter | Type         | Required | Description                            |
| --------- | ------------ | -------- | -------------------------------------- |
| height    | unsigned int | Yes      | Starting block height for the sum      |
| count     | unsigned int | Yes      | Number of blocks to include in the sum |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "get_coinbase_tx_sum",
    "params": {
        "height": 1563078,
        "count": 2
    },
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "get_coinbase_tx_sum",
    "params": {
        "height": 1563078,
        "count": 2
    },
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
    "method": "get_coinbase_tx_sum",
    "params": {
        "height": 1563078,
        "count": 2
    },
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
        "method": "get_coinbase_tx_sum",
        "params": {
                "height": 1563078,
                "count": 2
        },
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
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "emission_amount": 9387867431838,
        "emission_amount_top64": 0,
        "fee_amount": 83981287,
        "fee_amount_top64": 0,
        "status": "OK",
        "untrusted": false,
        "wide_emission_amount": "0x88981f9080e",
        "wide_fee_amount": "0x501b727"
    }
}
```
{% endcode %}

## Response Parameters

| Field                         | Type         | Description                                                      |
| ----------------------------- | ------------ | ---------------------------------------------------------------- |
| result.emission\_amount       | unsigned int | Cumulative coinbase emission over the range (atomic units)       |
| result.fee\_amount            | unsigned int | Cumulative transaction fees over the range (atomic units)        |
| result.wide\_emission\_amount | string       | 128-bit emission amount as hex (for ranges that overflow uint64) |
| result.wide\_fee\_amount      | string       | 128-bit fee amount as hex                                        |

## Use Cases

* Tracking XMR emission over time
* Economic and supply analytics
* Tail-emission auditing post the main supply schedule

## Error Handling

| Status Code | Error Message     | Cause                                                |
| ----------- | ----------------- | ---------------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                  |
| -32602      | Invalid params    | Request parameters are missing or malformed          |
| -32601      | Method not found  | Method does not exist or is not enabled on this node |
| 429         | Too Many Requests | Rate limit exceeded for your plan                    |

## SDK Integration

{% tabs %}
{% tab title="monero-javascript" %}
```javascript
import monerojs from 'monero-javascript';

const daemon = await monerojs.connectToDaemonRpc('https://go.getblock.io/<ACCESS-TOKEN>/');

// Most methods are exposed directly on the daemon client. For raw access:
const result = await daemon.getDaemonConnection().sendJsonRequest('get_coinbase_tx_sum', {"height": 1563078, "count": 2});
console.log(result);
```
{% endtab %}

{% tab title="monero-python" %}
```python
from monero.backends.jsonrpc import JSONRPCDaemon
from monero.daemon import Daemon

backend = JSONRPCDaemon(host='go.getblock.io', port=443, path='/<ACCESS-TOKEN>/', protocol='https')
daemon = Daemon(backend)

# For methods covered by the typed API, use the daemon attribute directly.
# For raw RPC access:
result = backend.raw_request('get_coinbase_tx_sum', {"height": 1563078, "count": 2})
print(result)
```
{% endtab %}
{% endtabs %}
