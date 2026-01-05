---
description: >-
  Example code for the getEvents JSON-RPC method. Complete guide on how to use
  getEvents JSON-RPC in GetBlock Web3 documentation.
---

# getEvents - Stellar

This method returns contract events from the Stellar network based on filter criteria.

### Parameters

| Parameter   | Type    | Description                       |
| ----------- | ------- | --------------------------------- |
| startLedger | integer | The first ledger to include       |
| filters     | array   | (optional) Array of event filters |
| pagination  | object  | (optional) Pagination options     |

**Filter Object:**

| Field       | Type   | Description                                       |
| ----------- | ------ | ------------------------------------------------- |
| type        | string | Event type: "contract", "system", or "diagnostic" |
| contractIds | array  | Array of contract IDs to filter                   |
| topics      | array  | Array of topic filters                            |

### Request

{% tabs %}
{% tab title="curl" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getEvents",
    "params": {
        "startLedger": 2539600,
        "filters": [
            {
                "type": "contract",
                "contractIds": ["CCJZ5DGASBWQXR5MPFCJXMBI333XE5U3FSJTNQU7RIKE3P5GN2K2WYD5"]
            }
        ],
        "pagination": {
            "limit": 100
        }
    },
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="index.js" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "getEvents",
    "params": {
        "startLedger": 2539600,
        "filters": [
            {
                "type": "contract",
                "contractIds": ["CCJZ5DGASBWQXR5MPFCJXMBI333XE5U3FSJTNQU7RIKE3P5GN2K2WYD5"]
            }
        ],
        "pagination": {
            "limit": 100
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
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "getEvents",
    "params": {
        "startLedger": 2539600,
        "filters": [
            {
                "type": "contract",
                "contractIds": ["CCJZ5DGASBWQXR5MPFCJXMBI333XE5U3FSJTNQU7RIKE3P5GN2K2WYD5"]
            }
        ],
        "pagination": {
            "limit": 100
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
            "method": "getEvents",
            "params": {
                "startLedger": 2539600,
                "filters": [
                    {
                        "type": "contract",
                        "contractIds": ["CCJZ5DGASBWQXR5MPFCJXMBI333XE5U3FSJTNQU7RIKE3P5GN2K2WYD5"]
                    }
                ],
                "pagination": {
                    "limit": 100
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
{% endtab %}
{% endtabs %}

### Response

{% code overflow="wrap" %}
```json
{
  "jsonrpc": "2.0",
  "id": 8675309,
  "result": {
    "events": [
      {
        "type": "contract",
        "ledger": 200010,
        "ledgerClosedAt": "2025-06-30T07:27:13Z",
        "contractId": "CDLZFC3SYJYDZT7K67VZ75HPJVIEUVNIXF47ZG2FB2RMQQVU2HHGCYSC",
        "id": "0000859036408881152-0000000003",
        "pagingToken": "0000859036408881152-0000000003",
        "inSuccessfulContractCall": true,
        "txHash": "d9e771ac73ec80503c7594f540d10ec068fb80981d11acea41aa193b7543c5ce",
        "topic": [
          "AAAADwAAAAh0cmFuc2Zlcg==",
          "AAAAEgAAAAAAAAAA6qNYcgGe/Zw2XRAUKPzIjtK2Cfp0eT8bn/BCJTcEq4s=",
          "AAAAEgAAAAGWF5MS3cqvdZjF5BY4yqI44bey/KmQmH9oF0gX3IIWuw==",
          "AAAADgAAAAZuYXRpdmUAAA=="
        ],
        "value": "AAAACgAAAAAAAAAAAAAAAF2UTIA="
      }
    ],
    "latestLedger": 320543,
    "cursor": "0000863490289963008-0000000010"
  }
}
```
{% endcode %}

### Response Parameters

| Field        | Type    | Description                     |
| ------------ | ------- | ------------------------------- |
| events       | array   | Array of event objects          |
| type         | string  | Event type                      |
| ledger       | integer | Ledger sequence number          |
| contractId   | string  | Contract that emitted the event |
| topic        | array   | Event topics (base64 XDR)       |
| value        | string  | Event value (base64 XDR)        |
| latestLedger | integer | Latest available ledger         |

### Use Case

The `getEvents` method is essential for:

* Smart contract event monitoring
* DApp state synchronization
* Event-driven applications
* Activity tracking
* Notification systems
* Analytics and indexing

### Error Handling

| Status Code | Error Message  | Cause                                   |
| ----------- | -------------- | --------------------------------------- |
| 403         | Forbidden      | Missing or invalid ACCESS-TOKEN         |
| -32602      | Invalid params | Invalid filter or pagination parameters |
