---
description: >-
  Example code for the getLedgerEntries JSON-RPC method. Complete guide on how
  to use getLedgerEntries JSON-RPC in GetBlock Web3 documentation.
---

# getLedgerEntries - Stellar

This method returns ledger entries for the specified keys, allowing you to read current state from the Stellar network.

### Parameters

| Parameter | Type  | Description                                     |
| --------- | ----- | ----------------------------------------------- |
| keys      | array | Array of ledger entry keys (base64-encoded XDR) |

### Request examples

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getLedgerEntries",
    "params": {
        "keys": [
            "AAAAAAAAAACp3BPIqFxM9XSnW6aHvavD3GWlJGfuylOt5tZL6CQtdQ=="
        ]
    },
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (axios)" %}
{% code title="example.js" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "getLedgerEntries",
    "params": {
        "keys": [
            "AAAAAAAAAACp3BPIqFxM9XSnW6aHvavD3GWlJGfuylOt5tZL6CQtdQ=="
        ]
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

{% tab title="Python (requests)" %}
{% code title="example.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "getLedgerEntries",
    "params": {
        "keys": [
            "AAAAAAAAAACp3BPIqFxM9XSnW6aHvavD3GWlJGfuylOt5tZL6CQtdQ=="
        ]
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
            "method": "getLedgerEntries",
            "params": {
                "keys": [
                    "AAAAAAAAAACp3BPIqFxM9XSnW6aHvavD3GWlJGfuylOt5tZL6CQtdQ=="
                ]
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
        "entries": [
            {
                "key": "AAAAAAAAAACp3BPIqFxM9XSnW6aHvavD3GWlJGfuylOt5tZL6CQtdQ==",
                "xdr": "AAAAAAAAAABp3BPIqFxM9XSnW6aHvavD3GWlJGfuylOt5tZL6CQtdQAAAABZYC8=",
                "lastModifiedLedgerSeq": 2539600,
                "liveUntilLedgerSeq": 2600000
            }
        ],
        "latestLedger": 2539605
    }
}
```

### Response Parameters

| Field                 | Type    | Description                                 |
| --------------------- | ------- | ------------------------------------------- |
| entries               | array   | Array of ledger entry objects               |
| key                   | string  | The ledger key (base64-encoded XDR)         |
| xdr                   | string  | The ledger entry value (base64-encoded XDR) |
| lastModifiedLedgerSeq | integer | Last ledger where entry was modified        |
| liveUntilLedgerSeq    | integer | Ledger until which the entry lives          |
| latestLedger          | integer | Latest ledger sequence number               |

### Use Case

The `getLedgerEntries` method is essential for:

* Reading account balances and sequence numbers
* Querying smart contract state
* Reading trustlines and offers
* Contract data retrieval
* State verification
* Application data loading

### Error Handling

| Status Code | Error Message  | Cause                           |
| ----------- | -------------- | ------------------------------- |
| 403         | Forbidden      | Missing or invalid ACCESS-TOKEN |
| -32602      | Invalid params | Invalid key format              |
