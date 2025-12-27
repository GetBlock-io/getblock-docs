---
description: >-
  Example code for the getnetworkhashps JSON-RPC method. Complete guide on how
  to use getnetworkhashps JSON-RPC in GetBlock Web3 documentation.
---

# getnetworkhashps - Bitcoin

This method returns the estimated network hashes per second based on the last n blocks.

### Parameters

| Parameter | Type   | Required | Description                                                                                      |
| --------- | ------ | -------- | ------------------------------------------------------------------------------------------------ |
| nblocks   | number | No       | Number of blocks to average over (default: 120). Use -1 to average since last difficulty change. |
| height    | number | No       | Block height to estimate at (default: -1 for current tip).                                       |

### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getnetworkhashps",
    "params": [120, -1],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "getnetworkhashps",
    "params": [120, -1],
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
{% endtab %}

{% tab title="Request" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "getnetworkhashps",
    "params": [120, -1],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endtab %}

{% tab title="Rust" %}
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
            "method": "getnetworkhashps",
            "params": [120, -1],
            "id": "getblock.io"
        }"#)
        .send()
        .await?;
    
    println!("{}", response.text().await?);
    Ok(())
}
```
{% endtab %}
{% endtabs %}

### Response

```json
{
    "result": 550000000000000000000,
    "error": null
}
```

### Response Parameters

| Field  | Type   | Description                                     |
| ------ | ------ | ----------------------------------------------- |
| result | number | Estimated hashes per second (in hashes/second). |

### Use Case

The `getnetworkhashps` method is essential for:

* Calculating network security metrics
* Estimating mining profitability
* Tracking hashrate trends over time
* Building mining analytics dashboards
* Monitoring network health
* Analyzing historical hashrate data

### Error Handling

| Status Code | Error Message             | Cause                            |
| ----------- | ------------------------- | -------------------------------- |
| 403         | Forbidden                 | Missing or invalid ACCESS-TOKEN. |
| -8          | Block height out of range | Specified height doesn't exist.  |

### Integration With Web3

The `getnetworkhashps` method helps developers:

* Build hashrate monitoring tools
* Create mining profitability calculators
* Implement network security indicators
* Track historical hashrate trends
* Support mining pool dashboards
