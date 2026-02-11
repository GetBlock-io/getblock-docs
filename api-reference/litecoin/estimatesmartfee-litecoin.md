---
description: >-
  Example code for the estimatesmartfee JSON-RPC method. Complete guide on how
  to use estimatesmartfee JSON-RPC in GetBlock Web3 documentation.
---

# estimatesmartfee - Litecoin

Estimates the approximate fee per kilobyte needed for a transaction to begin confirmation within conf\_target blocks.

### Parameters

| Parameter      | Type   | Description                                      |
| -------------- | ------ | ------------------------------------------------ |
| conf\_target   | number | Confirmation target in blocks (1 - 1008).        |
| estimate\_mode | string | Optional. Mode: UNSET, ECONOMICAL, CONSERVATIVE. |

### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "estimatesmartfee",
    "params": [6],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios (JavaScript)" %}
{% code title="axios.js" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "estimatesmartfee",
    "params": [6],
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

{% tab title="Python" %}
{% code title="request.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "estimatesmartfee",
    "params": [6],
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
{% code title="main.rs" %}
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
            "method": "estimatesmartfee",
            "params": [6],
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
    "result": {
        "feerate": 0.00000998,
        "blocks": 6
    },
    "error": null,
    "id": "getblock.io"
}
```
{% endcode %}

### Response Parameters

| Field   | Type   | Description                            |
| ------- | ------ | -------------------------------------- |
| feerate | number | Estimate fee rate in LTC/kB.           |
| blocks  | number | Block number where estimate was found. |

### Use Case

The `estimatesmartfee` method is essential for:

* Fee estimation
* Transaction optimization
* Wallet fee selection
* Cost prediction

### Error Handling

| Status Code | Error Message | Cause                            |
| ----------- | ------------- | -------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS-TOKEN. |

### Integration Notes

The `estimatesmartfee` method helps developers build robust applications that interact with the Litecoin blockchain.
