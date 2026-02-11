---
description: >-
  Example code for the testmempoolaccept JSON-RPC method. Complete guide on how
  to use testmempoolaccept JSON-RPC in GetBlock Web3 documentation.
---

# testmempoolaccept - Litecoin

Returns result of mempool acceptance tests indicating if raw transaction would be accepted by mempool.

### Parameters

| Parameter | Type  | Description                                  |
| --------- | ----- | -------------------------------------------- |
| rawtxs    | array | An array of hex strings of raw transactions. |

### Request examples

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "testmempoolaccept",
    "params": [["0100000001..."]],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios (JavaScript)" %}
{% code title="example.js" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "testmempoolaccept",
    "params": [["0100000001..."]],
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
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "testmempoolaccept",
    "params": [["0100000001..."]],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endtab %}

{% tab title="Rust (reqwest)" %}
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
            "method": "testmempoolaccept",
            "params": [["0100000001..."]],
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
    "result": [{
        "txid": "txid...",
        "allowed": true,
        "vsize": 144,
        "fees": {
            "base": 0.00001000
        }
    }]
}
```

### Response Parameters

| Field   | Type    | Description                                  |
| ------- | ------- | -------------------------------------------- |
| txid    | string  | The transaction hash.                        |
| allowed | boolean | If the mempool would accept the transaction. |
| vsize   | number  | Virtual transaction size.                    |

### Use Case

The `testmempoolaccept` method is essential for:

* Transaction validation
* Pre-broadcast testing
* Fee verification
* Error checking

### Error Handling

| Status Code | Error Message | Cause                            |
| ----------- | ------------- | -------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS-TOKEN. |

### Integration Notes

The `testmempoolaccept` method helps developers build robust applications that interact with the Litecoin blockchain.
