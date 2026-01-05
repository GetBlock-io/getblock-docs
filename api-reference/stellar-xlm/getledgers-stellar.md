---
description: >-
  Example code for the getLedgers JSON-RPC method. Complete guide on how to use
  getLedgers JSON-RPC in GetBlock Web3 documentation.
---

# getLedgers - Stellar

This method returns a list of ledgers from the Stellar network within a specified range.

### Parameters

| Parameter   | Type    | Description                                 |
| ----------- | ------- | ------------------------------------------- |
| startLedger | integer | The first ledger sequence number to include |
| pagination  | object  | (optional) Pagination options               |

Pagination Object:

| Field  | Type    | Description                         |
| ------ | ------- | ----------------------------------- |
| cursor | string  | Cursor for pagination               |
| limit  | integer | Maximum number of ledgers to return |

### Request

{% tabs %}
{% tab title="cURL" %}
{% code title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getLedgers",
    "params": {
        "startLedger": 2539600,
        "pagination": {
            "limit": 5
        }
    },
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="axios example" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "getLedgers",
    "params": {
        "startLedger": 2539600,
        "pagination": {
            "limit": 5
        }
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
{% code title="requests example" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "getLedgers",
    "params": {
        "startLedger": 2539600,
        "pagination": {
            "limit": 5
        }
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
{% code title="reqwest example" %}
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
            "method": "getLedgers",
            "params": {
                "startLedger": 2539600,
                "pagination": {
                    "limit": 5
                }
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

{% code title="Example response" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "ledgers": [
            {
                "hash": "abc123...",
                "sequence": 2539600,
                "ledgerCloseTime": "1700159337"
            }
        ],
        "latestLedger": 2539605,
        "latestLedgerCloseTime": "1700159362",
        "oldestLedger": 2522324,
        "oldestLedgerCloseTime": "1700072999",
        "cursor": "2539604"
    }
}
```
{% endcode %}

### Response Parameters

| Field                 | Type    | Description                           |
| --------------------- | ------- | ------------------------------------- |
| ledgers               | array   | Array of ledger objects               |
| latestLedger          | integer | Latest ledger sequence                |
| latestLedgerCloseTime | string  | Unix timestamp of latest ledger close |
| oldestLedger          | integer | Oldest available ledger sequence      |
| cursor                | string  | Cursor for pagination                 |

### Use Case

The `getLedgers` method is essential for:

* Block explorer functionality
* Historical data analysis
* Chain synchronization
* Ledger metadata retrieval
* Network activity monitoring
* Data indexing services

### Error Handling

| Status Code | Error Message  | Cause                              |
| ----------- | -------------- | ---------------------------------- |
| 403         | Forbidden      | Missing or invalid ACCESS-TOKEN    |
| -32602      | Invalid params | Invalid ledger range or pagination |
