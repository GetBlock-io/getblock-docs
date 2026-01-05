---
description: >-
  Example code for the getHealth JSON-RPC method. Complete guide on how to use
  getHealth JSON-RPC in GetBlock Web3 documentation.
---

# getHealth - Stellar

This method returns the health status of the Stellar RPC node.

### Parameters

* None

### Request

{% tabs %}
{% tab title="curl" %}
{% code title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getHealth",
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (axios)" %}
{% code title="getHealth.js" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "getHealth",
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

{% tab title="Python (requests)" %}
{% code title="getHealth.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "getHealth",
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

{% tab title="Rust (reqwest)" %}
{% code title="getHealth.rs" %}
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
            "method": "getHealth",
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

{% code title="response.json" %}
```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "status": "healthy",
        "latestLedger": 60624213,
        "oldestLedger": 60503254,
        "ledgerRetentionWindow": 120960
    }
}
```
{% endcode %}

### Response Parameters

| Field                 | Type    | Description                                           |
| --------------------- | ------- | ----------------------------------------------------- |
| status                | string  | Health status ("healthy" or "unhealthy")              |
| latestLedger          | integer | The sequence number of the latest ledger              |
| oldestLedger          | integer | The sequence number of the oldest ledger in retention |
| ledgerRetentionWindow | integer | Number of ledgers retained                            |

### Use Case

The `getHealth` method is essential for:

* Node health monitoring
* Service availability checks
* Infrastructure monitoring dashboards
* Load balancer health probes
* Application readiness verification
* Alerting systems

### Error Handling

| Status Code | Error Message       | Cause                           |
| ----------- | ------------------- | ------------------------------- |
| 403         | Forbidden           | Missing or invalid ACCESS-TOKEN |
| 503         | Service Unavailable | Node is unhealthy or syncing    |
