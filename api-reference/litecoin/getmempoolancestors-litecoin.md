---
description: >-
  Example code for the getmempoolancestors JSON-RPC method. Complete guide on
  how to use getmempoolancestors JSON-RPC in GetBlock Web3 documentation.
---

# getmempoolancestors - Litecoin

If the txid is in the mempool, this returns all in-mempool ancestors.

### Parameters

| Parameter | Type    | Description                                    |
| --------- | ------- | ---------------------------------------------- |
| txid      | string  | The transaction id (must be in mempool).       |
| verbose   | boolean | Optional. True for JSON object. Default=false. |

### Request examples

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getmempoolancestors",
    "params": ["txid123...", false],
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
    "method": "getmempoolancestors",
    "params": ["txid123...", false],
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
    "method": "getmempoolancestors",
    "params": ["txid123...", false],
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
            "method": "getmempoolancestors",
            "params": ["txid123...", false],
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
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": ["ancestor_txid1", "ancestor_txid2"]
}
```
{% endcode %}

### Response Parameters

| Field  | Type  | Description                        |
| ------ | ----- | ---------------------------------- |
| result | array | Array of ancestor transaction IDs. |

### Use Case

The `getmempoolancestors` method is essential for:

* Dependency tracking
* CPFP analysis
* Transaction chains
* Fee bumping

### Error Handling

| Status Code | Error Message | Cause                            |
| ----------- | ------------- | -------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS-TOKEN. |

### Integration Notes

The `getmempoolancestors` method helps developers build robust applications that interact with the Litecoin blockchain.
