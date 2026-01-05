---
description: >-
  Example code for the getFeeStats JSON-RPC method. Complete guide on how to use
  getFeeStats JSON-RPC in GetBlock Web3 documentation.
---

# getFeeStats - Stellar

This method returns statistics about charged inclusion fees for Soroban transactions and Stellar transactions.

### Parameters

* None

### Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getFeeStats",
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code overflow="wrap" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "getFeeStats",
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
    .then(response => console.log(JSON.stringify(response.data)))
    .catch(error => console.log(error));
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code overflow="wrap" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "getFeeStats",
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code overflow="wrap" %}
```rust
use reqwest::header;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = reqwest::Client::new();
    
    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header(header::CONTENT_TYPE, "application/json")
        .body(r#"{
            "jsonrpc": "2.0",
            "method": "getFeeStats",
            "id": "getblock.io"
        }"#)
        .send()
        .await?;
    
    println!("{}", response.text().await?);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Response

{% code title="Response (example)" %}
```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "sorobanInclusionFee": {
            "max": "200",
            "min": "100",
            "mode": "200",
            "p10": "200",
            "p20": "200",
            "p30": "200",
            "p40": "200",
            "p50": "200",
            "p60": "200",
            "p70": "200",
            "p80": "200",
            "p90": "200",
            "p95": "200",
            "p99": "200",
            "transactionCount": "4920",
            "ledgerCount": 50
        },
        "inclusionFee": {
            "max": "200",
            "min": "100",
            "mode": "100",
            "p10": "100",
            "p20": "100",
            "p30": "100",
            "p40": "100",
            "p50": "100",
            "p60": "100",
            "p70": "100",
            "p80": "100",
            "p90": "100",
            "p95": "100",
            "p99": "200",
            "transactionCount": "1860",
            "ledgerCount": 10
        },
        "latestLedger": 60624208
    }
}
```
{% endcode %}

### Response Parameters

| Field               | Type    | Description                                            |
| ------------------- | ------- | ------------------------------------------------------ |
| sorobanInclusionFee | object  | Fee statistics for Soroban smart contract transactions |
| inclusionFee        | object  | Fee statistics for classic Stellar transactions        |
| latestLedger        | integer | Sequence number of the latest ledger                   |
| max                 | string  | Maximum fee observed                                   |
| min                 | string  | Minimum fee observed                                   |
| mode                | string  | Most common fee                                        |
| p10-p99             | string  | Percentile fee values                                  |

### Use Case

The `getFeeStats` method is essential for:

* Fee estimation for transactions
* Gas price optimization
* Transaction cost planning
* Network congestion monitoring
* Wallet fee suggestions
* DApp cost optimization

### Error Handling

| Status Code | Error Message | Cause                           |
| ----------- | ------------- | ------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS-TOKEN |
