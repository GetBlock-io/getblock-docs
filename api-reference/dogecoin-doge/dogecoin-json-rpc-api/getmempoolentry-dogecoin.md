---
description: >-
  Example code for the getmempoolentry JSON-RPC method. Complete guide on how to use
  getmempoolentry JSON-RPC in GetBlock Web3 documentation.
---

# getmempoolentry - Dogecoin

This method returns mempool data for a single transaction that is currently queued.

## Parameters

| Parameter | Type   | Required | Description                              |
| --------- | ------ | -------- | ---------------------------------------- |
| txid      | string | Yes      | The transaction id of a mempool entry    |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getmempoolentry",
    "params": ["d0dd61c93cd4a571030c15b5f6a94466a8f7246b986dd318d656c910e6e5a8ed"],
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
  jsonrpc: '2.0', method: 'getmempoolentry', params: ["d0dd61c93cd4a571030c15b5f6a94466a8f7246b986dd318d656c910e6e5a8ed"], id: 'getblock.io'
});
console.log(data.result);
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
        'method': 'getmempoolentry',
        'params': ["d0dd61c93cd4a571030c15b5f6a94466a8f7246b986dd318d656c910e6e5a8ed"],
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
            "method": "getmempoolentry",
            "params": ["d0dd61c93cd4a571030c15b5f6a94466a8f7246b986dd318d656c910e6e5a8ed"],
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
        "size": 225,
        "fee": 0.00340500,
        "modifiedfee": 0.00340500,
        "time": 1789703820,
        "height": 6378929,
        "descendantcount": 1,
        "descendantsize": 225,
        "ancestorcount": 1,
        "ancestorsize": 225,
        "depends": []
    }
}
```

## Response Parameters

| Parameter | Type         | Description                                      |
| --------- | ------------ | ------------------------------------------------ |
| error     | null\|object | Error object when the call fails, otherwise null |
| id        | string       | Request identifier matching the request          |
| result.size | numeric | Serialized transaction size in bytes |
| result.fee | numeric | Fee paid, in DOGE |
| result.time | numeric | Time the transaction entered the mempool |
| result.height | numeric | Chain height when the transaction entered the mempool |
| result.descendantcount | numeric | Number of in-mempool descendants, including this transaction |
| result.ancestorcount | numeric | Number of in-mempool ancestors, including this transaction |
| result.depends | array | Transaction ids of unconfirmed parents |

## Use Cases

* **Pending Inspection**: Read the fee and size of a queued transaction
* **Chain Analysis**: Read `depends` to find unconfirmed parents
* **Stuck Diagnosis**: Compare a transaction's fee against the mempool minimum

## Error Handling

| Error Code | Message | Description |
| ---------- | ------- | ----------- |
| -5 | Transaction not in mempool | The transaction id is not currently queued |
| -32700 | Parse error | Request body is not valid JSON |
| -32603 | Internal error | Node failed to process the request |
