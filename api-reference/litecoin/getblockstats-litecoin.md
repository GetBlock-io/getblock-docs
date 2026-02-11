---
description: >-
  Example code for the getblockstats JSON-RPC method. Complete guide on how to
  use getblockstats JSON-RPC in GetBlock Web3 documentation.
---

# getblockstats - Litecoin

Compute per-block statistics for a given window.

### Parameters

| Parameter        | Type             | Description                               |
| ---------------- | ---------------- | ----------------------------------------- |
| hash\_or\_height | string or number | The block hash or height.                 |
| stats            | array            | Optional. Values to include. Default=all. |

### Request

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getblockstats",
    "params": [2750000],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios (JavaScript)" %}
{% code title="axios.js" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "getblockstats",
    "params": [2750000],
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
{% code title="request.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "getblockstats",
    "params": [2750000],
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
            "method": "getblockstats",
            "params": [2750000],
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
        "avgfee": 952,
        "avgfeerate": 5,
        "avgtxsize": 270,
        "blockhash": "2b27dd39d0d380791408561a3f11c45b191ce11f88940128702861409c902168",
        "feerate_percentiles": [
            1,
            1,
            1,
            3,
            10
        ],
        "height": 2750000,
        "ins": 1439,
        "maxfee": 132980,
        "maxfeerate": 375,
        "maxtxsize": 6863,
        "medianfee": 157,
        "mediantime": 1725424763,
        "mediantxsize": 238,
        "minfee": 0,
        "minfeerate": 0,
        "mintxsize": 97,
        "outs": 3210,
        "subsidy": 625000000,
        "swtotal_size": 283709,
        "swtotal_weight": 725729,
        "swtxs": 1094,
        "time": 1725425585,
        "total_out": 11021814089921,
        "total_size": 319109,
        "total_weight": 867317,
        "totalfee": 1122658,
        "txs": 1180,
        "utxo_increase": 1771,
        "utxo_size_inc": 115599
    },
    "error": null,
    "id": "getblock.io"
}
```
{% endcode %}

### Response Parameters

| Field    | Type   | Description                 |
| -------- | ------ | --------------------------- |
| height   | number | The block height.           |
| txs      | number | The number of transactions. |
| avgfee   | number | Average fee in satoshis.    |
| totalfee | number | Total fees in satoshis.     |

### Use Case

The `getblockstats` method is essential for:

* Block analysis
* Fee statistics
* Network metrics
* Mining analytics

### Error Handling

| Status Code | Error Message | Cause                            |
| ----------- | ------------- | -------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS-TOKEN. |

### Integration Notes

The `getblockstats` method helps developers build robust applications that interact with the Litecoin blockchain.
