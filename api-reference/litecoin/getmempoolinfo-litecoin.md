---
description: >-
  Example code for the getmempoolinfo JSON-RPC method. Complete guide on how to
  use getmempoolinfo JSON-RPC in GetBlock Web3 documentation.
---

# getmempoolinfo - Litecoin

Returns details on the active state of the TX memory pool.

### Parameters

* None

### Request examples

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
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

{% tab title="Axios (JavaScript)" %}
{% code title="axios example" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "getmempoolinfo",
    "params": [],
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
{% code title="python example" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "getmempoolinfo",
    "params": [],
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
{% code title="rust example" %}
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
            "method": "getmempoolinfo",
            "params": [],
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

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "loaded": true,
        "size": 1234,
        "bytes": 567890,
        "usage": 1234567,
        "maxmempool": 300000000,
        "mempoolminfee": 0.00001000,
        "minrelaytxfee": 0.00001000
    }
}
```

### Response Parameters

| Field         | Type   | Description                             |
| ------------- | ------ | --------------------------------------- |
| size          | number | Current tx count in mempool.            |
| bytes         | number | Sum of all tx sizes.                    |
| usage         | number | Total memory usage.                     |
| mempoolminfee | number | Minimum fee rate for tx to be accepted. |

### Use Case

{% hint style="info" %}
The `getmempoolinfo` method is essential for:

* Mempool monitoring
* Fee estimation
* Network congestion analysis
* Transaction timing optimization
{% endhint %}

### Error Handling

| Status Code | Error Message | Cause                            |
| ----------- | ------------- | -------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS-TOKEN. |

### Integration Notes

The `getmempoolinfo` method helps developers build robust applications that interact with the Litecoin blockchain.
