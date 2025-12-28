---
description: >-
  Example code for the getchaintxstats JSON-RPC method. Complete guide on how to
  use getchaintxstats JSON-RPC in GetBlock Web3 documentation.
---

# getchaintxstats - Bitcoin

This method computes statistics about the total number and rate of transactions in the chain.

### Parameters

| Parameter | Type   | Required | Description                                            |
| --------- | ------ | -------- | ------------------------------------------------------ |
| nblocks   | number | No       | Number of blocks to average over (default: one month). |
| blockhash | string | No       | Block hash for the end of the range.                   |

### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getchaintxstats",
    "params": [144],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "getchaintxstats",
    "params": [144],
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
    "method": "getchaintxstats",
    "params": [144],
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
            "method": "getchaintxstats",
            "params": [144],
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
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "time": 1700000000,
        "txcount": 950000000,
        "window_final_block_hash": "000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428",
        "window_final_block_height": 820000,
        "window_block_count": 144,
        "window_tx_count": 450000,
        "window_interval": 86400,
        "txrate": 5.208333333
    }
}
```

### Response Parameters

| Field                        | Type   | Description                                             |
| ---------------------------- | ------ | ------------------------------------------------------- |
| time                         | number | Timestamp of the last block in the window.              |
| txcount                      | number | Total number of transactions in chain up to that point. |
| window\_final\_block\_hash   | string | Hash of the final block in the window.                  |
| window\_final\_block\_height | number | Height of the final block in the window.                |
| window\_block\_count         | number | Number of blocks in the window.                         |
| window\_tx\_count            | number | Number of transactions in the window.                   |
| window\_interval             | number | Time span of the window in seconds.                     |
| txrate                       | number | Average rate of transactions per second in the window.  |

### Use Case

The `getchaintxstats` method is essential for:

* Monitoring network transaction throughput
* Building blockchain analytics dashboards
* Tracking adoption metrics
* Analyzing network capacity utilization
* Creating transaction rate visualizations
* Reporting on blockchain health

### Error Handling

| Status Code | Error Message                | Cause                                   |
| ----------- | ---------------------------- | --------------------------------------- |
| 403         | Forbidden                    | Missing or invalid ACCESS-TOKEN.        |
| -5          | Block not found              | The specified blockhash does not exist. |
| -8          | nblocks is larger than chain | Requested more blocks than available.   |

### Integration Notes

The `getchaintxstats` method helps developers:

* Build transaction rate monitors
* Create network analytics tools
* Track blockchain adoption
* Implement capacity planning systems
* Generate network health reports
