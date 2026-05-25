# get\_output\_histogram monero

This method returns a histogram of RingCT outputs by amount. Wallets use this data to construct ring signatures with appropriate decoys.

## Parameters

| Parameter      | Type                  | Required | Description                                                             |
| -------------- | --------------------- | -------- | ----------------------------------------------------------------------- |
| amounts        | array of unsigned int | No       | Specific amounts to bucket. Defaults to all                             |
| min\_count     | unsigned int          | No       | Minimum count per bucket                                                |
| max\_count     | unsigned int          | No       | Maximum count per bucket                                                |
| unlocked       | boolean               | No       | If `true`, only count unlocked outputs                                  |
| recent\_cutoff | unsigned int          | No       | If non-zero, also report buckets with outputs since this Unix timestamp |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "get_output_histogram",
    "params": {
        "amounts": [
            20000000000
        ],
        "min_count": 0,
        "max_count": 0,
        "unlocked": true,
        "recent_cutoff": 0
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
    "method": "get_output_histogram",
    "params": {
        "amounts": [
            20000000000
        ],
        "min_count": 0,
        "max_count": 0,
        "unlocked": true,
        "recent_cutoff": 0
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
    "method": "get_output_histogram",
    "params": {
        "amounts": [
            20000000000
        ],
        "min_count": 0,
        "max_count": 0,
        "unlocked": true,
        "recent_cutoff": 0
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
        "method": "get_output_histogram",
        "params": {
                "amounts": [
                        20000000000
                ],
                "min_count": 0,
                "max_count": 0,
                "unlocked": true,
                "recent_cutoff": 0
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
        "histogram": [
            {
                "amount": 20000000000,
                "recent_instances": 0,
                "total_instances": 381458,
                "unlocked_instances": 381458
            }
        ],
        "status": "OK",
        "untrusted": false
    }
}
```
{% endcode %}

## Response Parameters

| Field                                   | Type         | Description                                       |
| --------------------------------------- | ------------ | ------------------------------------------------- |
| result.histogram                        | array        | Array of histogram buckets                        |
| result.histogram\[].amount              | unsigned int | Output amount in atomic units                     |
| result.histogram\[].total\_instances    | unsigned int | Total outputs of this amount on chain             |
| result.histogram\[].unlocked\_instances | unsigned int | Of the above, how many are unlocked and spendable |
| result.histogram\[].recent\_instances   | unsigned int | Recent outputs (if `recent_cutoff` was supplied)  |

## Use Cases

* Wallet decoy selection for ring signatures
* Anonymity-set research and analytics
* Auditing chain composition over time

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
const result = await daemon.getDaemonConnection().sendJsonRequest('get_output_histogram', {"amounts": [20000000000], "min_count": 0, "max_count": 0, "unlocked": true, "recent_cutoff": 0});
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
result = backend.raw_request('get_output_histogram', {"amounts": [20000000000], "min_count": 0, "max_count": 0, "unlocked": true, "recent_cutoff": 0})
print(result)
```
{% endtab %}
{% endtabs %}
