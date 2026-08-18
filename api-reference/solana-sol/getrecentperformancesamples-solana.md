---
description: >-
  Example code for the getRecentPerformanceSamples JSON-RPC method. Complete
  guide on how to use the getRecentPerformanceSamples JSON-RPC method in the
  GetBlock Web3 documentation.
---

# getRecentPerformanceSamples - Solana

This method returns performance samples taken over recent 60-second windows, including transaction counts and slots elapsed. It is the standard source for calculating observed throughput.

## Parameters

| Parameter | Type   | Required | Description                                             |
| --------- | ------ | -------- | ------------------------------------------------------- |
| limit     | number | No       | Number of samples to return, up to 720. Defaults to 720 |

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getRecentPerformanceSamples",
    "params": [2],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="@solana/web3.js" %}
```javascript
const { Connection } = require('@solana/web3.js');

const connection = new Connection('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', 'confirmed');

const samples = await connection.getRecentPerformanceSamples(2);

console.log(samples[0].numTransactions / samples[0].samplePeriodSecs);
```
{% endtab %}

{% tab title="Request" %}
```python
import requests

response = requests.post(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'getRecentPerformanceSamples',
        'params': [2],
        'id': 'getblock.io'
    }
)

print(response.json()['result'])
```
{% endtab %}

{% tab title="Rust" %}
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
            "method": "getRecentPerformanceSamples",
            "params": [2],
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
{% endtab %}
{% endtabs %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": [
        {
            "numNonVoteTransactions": 68412,
            "numSlots": 149,
            "numTransactions": 182934,
            "samplePeriodSecs": 60,
            "slot": 397234561
        }
    ]
}
```

## Response Parameters

| Parameter | Type   | Description                                            |
| --------- | ------ | ------------------------------------------------------ |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                      |
| id        | string | Request identifier matching the request                |
| result    | array  | Array of performance sample objects, most recent first |

### Sample Entry

| Field                  | Type   | Description                                           |
| ---------------------- | ------ | ----------------------------------------------------- |
| slot                   | number | Slot the sample was taken at                          |
| numTransactions        | number | Total transactions processed during the sample period |
| numNonVoteTransactions | number | Transactions excluding validator vote transactions    |
| numSlots               | number | Slots completed during the sample period              |
| samplePeriodSecs       | number | Length of the sample window in seconds                |

## Use Cases

* **True TPS**: Divide numNonVoteTransactions by samplePeriodSecs to exclude vote traffic
* **Congestion Signals**: Raise priority fees when recent samples show sustained load
* **Status Pages**: Chart throughput and slot rate on a public network dashboard
* **Capacity Planning**: Compare peak and baseline throughput across 12 hours of samples

## Error Handling

| Error Code | Message        | Description                                      |
| ---------- | -------------- | ------------------------------------------------ |
| -32602     | Invalid params | Limit exceeds 720 or is not a positive integer   |
| -32603     | Internal error | Node failed to read the performance sample store |
