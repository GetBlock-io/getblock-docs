---
description: >-
  Example code for the getmempoolinfo JSON-RPC method. Complete guide on how to
  use the getmempoolinfo JSON-RPC method in the GetBlock Web3 documentation.
---

# getmempoolinfo - Bitcoin Cash

This method returns details about the active state of the mempool, including transaction count, total size, and the minimum fee required for acceptance.

## Parameters

This method does not accept any parameters.

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getmempoolinfo",
    "params": [],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="bitcoinjs-lib" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const { data } = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
  jsonrpc: '2.0', method: 'getmempoolinfo', params: [], id: 'getblock.io'
});
console.log(data.result.size, data.result.mempoolminfee);
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
        'method': 'getmempoolinfo',
        'params': [],
        'id': 'getblock.io'
    }
)

print(response.json()['result'])
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
            "method": "getmempoolinfo",
            "params": [],
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
    "error": null,
    "id": "getblock.io",
    "result": {
        "loaded": true,
        "size": 6334,
        "bytes": 3892014,
        "usage": 18420992,
        "maxmempool": 300000000,
        "mempoolminfee": 1e-05,
        "minrelaytxfee": 1e-05
    }
}
```

## Response Parameters

| Parameter | Type         | Description                                      |
| --------- | ------------ | ------------------------------------------------ |
| error     | null\|object | Error object when the call fails, otherwise null |
| id        | string       | Request identifier matching the request          |
| result    | object       | Object describing the current mempool state      |

### Result Object

| Field         | Type    | Description                                    |
| ------------- | ------- | ---------------------------------------------- |
| size          | numeric | Number of transactions in the mempool          |
| bytes         | numeric | Total size of mempool transactions in bytes    |
| usage         | numeric | Total memory usage of the mempool in bytes     |
| maxmempool    | numeric | Maximum memory usage allowed for the mempool   |
| mempoolminfee | numeric | Minimum fee rate for acceptance, in BCH per kB |

## Use Cases

* **Fee Floors**: Read mempoolminfee before constructing a transaction
* **Backlog Monitoring**: Alert when the mempool approaches its size limit
* **Capacity Dashboards**: Display mempool depth on a network status page
* **Relay Policy**: Check minrelaytxfee to avoid building non-relayable transactions

## Error Handling

| Error Code | Message         | Description                                   |
| ---------- | --------------- | --------------------------------------------- |
| -32700     | Parse error     | Request body is not valid JSON                |
| -32600     | Invalid request | The JSON sent is not a valid request object   |
| -32603     | Internal error  | Node failed to read the requested chain state |
