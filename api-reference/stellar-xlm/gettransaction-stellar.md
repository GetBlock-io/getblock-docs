---
description: >-
  Example code for the getTransaction JSON-RPC method. Complete guide on how to
  use getTransaction JSON-RPC in GetBlock Web3 documentation.
---

# getTransaction - Stellar

This method returns details about a specific transaction by its hash on the Stellar network.

### Parameters

| Parameter | Type   | Description                 |
| --------- | ------ | --------------------------- |
| hash      | string | Transaction hash to look up |

### Request examples

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getTransaction",
    "params": {
        "hash": "84b4d00835996d6dc7c56ac601aeb0560b0ee00fe4f9495417bd2e0efa474902"
    },
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "getTransaction",
    "params": {
        "hash": "d8ec9b68780314ffdfdfc2194b1b35dd27d7303c3bceaef6447e31631a1419dc"
    },
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
{% code title="example.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "getTransaction",
    "params": {
        "hash": "d8ec9b68780314ffdfdfc2194b1b35dd27d7303c3bceaef6447e31631a1419dc"
    },
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
{% code title="example.rs" %}
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
            "method": "getTransaction",
            "params": {
                "hash": "d8ec9b68780314ffdfdfc2194b1b35dd27d7303c3bceaef6447e31631a1419dc"
            },
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
        "status": "SUCCESS",
        "latestLedger": 2553978,
        "latestLedgerCloseTime": "1700159337",
        "oldestLedger": 2536698,
        "oldestLedgerCloseTime": "1700072957",
        "ledger": 2553920,
        "createdAt": "1700159048",
        "applicationOrder": 1,
        "feeBump": false,
        "envelopeXdr": "AAAAAgAAAAAg4dbAxs...",
        "resultXdr": "AAAAAAAAAGT...",
        "resultMetaXdr": "AAAAAwAAAA..."
    }
}
```

### Response Parameters

| Field            | Type    | Description                                      |
| ---------------- | ------- | ------------------------------------------------ |
| status           | string  | Transaction status (SUCCESS, FAILED, NOT\_FOUND) |
| latestLedger     | integer | Latest ledger sequence                           |
| ledger           | integer | Ledger containing the transaction                |
| createdAt        | string  | Unix timestamp of transaction                    |
| applicationOrder | integer | Order in which tx was applied in ledger          |
| envelopeXdr      | string  | Transaction envelope (base64 XDR)                |
| resultXdr        | string  | Transaction result (base64 XDR)                  |
| resultMetaXdr    | string  | Transaction metadata (base64 XDR)                |

### Use Case

The `getTransaction` method is essential for:

* Transaction confirmation checking
* Payment verification
* Transaction debugging
* Receipt retrieval
* Transaction history analysis
* Error diagnosis

### Error Handling

| Status Code | Error Message  | Cause                           |
| ----------- | -------------- | ------------------------------- |
| 403         | Forbidden      | Missing or invalid ACCESS-TOKEN |
| -32602      | Invalid params | Invalid transaction hash format |
