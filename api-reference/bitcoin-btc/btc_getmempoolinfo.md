---
description: >-
  Example code for the getmempoolinfo JSON-RPC method. Complete guide on how to
  use getmempoolinfo JSON-RPC in GetBlock Web3 documentation.
---

# getmempoolinfo - Bitcoin

This method returns details on the active state of the TX memory pool.

### Parameters

* None

### Request

{% tabs %}
{% tab title="cURL" %}
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
{% endtab %}

{% tab title="Axios" %}
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
{% endtab %}

{% tab title="Request" %}
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
{% endtab %}
{% endtabs %}

### Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "loaded": true,
        "size": 15000,
        "bytes": 8500000,
        "usage": 52000000,
        "total_fee": 1.25000000,
        "maxmempool": 300000000,
        "mempoolminfee": 0.00001000,
        "minrelaytxfee": 0.00001000,
        "incrementalrelayfee": 0.00001000,
        "unbroadcastcount": 0,
        "fullrbf": false
    }
}
```

### Response Parameters

| Field               | Type    | Description                                                             |
| ------------------- | ------- | ----------------------------------------------------------------------- |
| loaded              | boolean | Whether the mempool is fully loaded.                                    |
| size                | number  | Current transaction count in mempool.                                   |
| bytes               | number  | Sum of all virtual transaction sizes in bytes.                          |
| usage               | number  | Total memory usage for the mempool.                                     |
| total\_fee          | number  | Total fees for all transactions in BTC.                                 |
| maxmempool          | number  | Maximum memory usage for the mempool in bytes.                          |
| mempoolminfee       | number  | Minimum fee rate (in BTC/kvB) for tx to be accepted.                    |
| minrelaytxfee       | number  | Minimum relay fee for transactions.                                     |
| incrementalrelayfee | number  | Minimum fee rate increment for mempool limiting or BIP 125 replacement. |
| unbroadcastcount    | number  | Number of transactions waiting to be broadcast.                         |
| fullrbf             | boolean | Whether full replace-by-fee signaling is enabled.                       |

### Use Case

The `getmempoolinfo` method is essential for:

* Monitoring mempool congestion levels
* Determining optimal transaction fees
* Building fee estimation algorithms
* Creating mempool visualization dashboards
* Monitoring network transaction volume
* Detecting mempool pressure situations

### Error Handling

| Status Code | Error Message | Cause                            |
| ----------- | ------------- | -------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS-TOKEN. |

### Integration With Web3

The `getmempoolinfo` method helps developers:

* Build mempool monitoring dashboards
* Implement dynamic fee calculators
* Create network congestion indicators
* Monitor node mempool health
* Track transaction backlog
