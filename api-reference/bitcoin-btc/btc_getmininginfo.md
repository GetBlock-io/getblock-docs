---
description: >-
  Example code for the getmininginfo JSON-RPC method. Complete guide on how to
  use getmininginfo JSON-RPC in GetBlock Web3 documentation.
---

# getmininginfo - Bitcoin

This method returns a JSON object containing mining-related information.

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
    "method": "getmininginfo",
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
    "method": "getmininginfo",
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
    "method": "getmininginfo",
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
            "method": "getmininginfo",
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
    "result": {
        "blocks": 928672,
        "currentblockweight": 3995956,
        "currentblocktx": 3955,
        "difficulty": 148195306640204.7,
        "networkhashps": 1.123192284545419e+21,
        "pooledtx": 20906,
        "chain": "main",
        "warnings": ""
    },
    "error": null
}
```

### Response Parameters

| Field         | Type   | Description                                 |
| ------------- | ------ | ------------------------------------------- |
| blocks        | number | Current block height.                       |
| difficulty    | number | Current network difficulty.                 |
| networkhashps | number | Estimated network hash rate per second.     |
| pooledtx      | number | Number of transactions in the mempool.      |
| chain         | string | Current network name (main, test, regtest). |
| warnings      | string | Any network or blockchain warnings.         |

### Use Case

The `getmininginfo` method is essential for:

* Monitoring mining network status
* Building mining dashboards
* Calculating mining profitability
* Tracking network hash rate
* Monitoring blockchain security
* Creating mining analytics tools

### Error Handling

| Status Code | Error Message | Cause                            |
| ----------- | ------------- | -------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS-TOKEN. |

### Integration with Web3

The `getmininginfo` method helps developers:

* Build mining monitoring applications
* Create profitability calculators
* Implement network health dashboards
* Track mining metrics over time
* Support mining pool integrations
