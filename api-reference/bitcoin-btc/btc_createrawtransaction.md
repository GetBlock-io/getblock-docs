---
description: >-
  Example code for the createrawtransaction JSON RPC method. Сomplete guide on
  how to use createrawtransaction JSON RPC in GetBlock Web3 documentation.
---

# createrawtransaction - Bitcoin

## createrawtransaction - Bitcoin

Example code for the createrawtransaction JSON-RPC method. Complete guide on how to use createrawtransaction JSON-RPC in GetBlock Web3 documentation.

This method creates a raw transaction, spending the given inputs and creating new outputs. The transaction is unsigned and must be signed before it can be broadcast.

### Parameters

| Parameter          | Type    | Required | Description                                                   |
| ------------------ | ------- | -------- | ------------------------------------------------------------- |
| inputs             | array   | Yes      | Array of input objects specifying UTXOs to spend.             |
| inputs\[].txid     | string  | Yes      | The transaction id of the UTXO to spend.                      |
| inputs\[].vout     | number  | Yes      | The output index of the UTXO to spend.                        |
| inputs\[].sequence | number  | No       | The sequence number (default: depends on locktime).           |
| outputs            | array   | Yes      | Array of output objects mapping addresses to amounts or data. |
| locktime           | number  | No       | Raw locktime. Non-0 value also activates inputs (default: 0). |
| replaceable        | boolean | No       | Marks this transaction as BIP125-replaceable (default: true). |

### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "createrawtransaction",
    "params": [
        [{"txid": "ee652f0b40209bd02468de0c6336854f5efdd79fb865560aef2c46f4fa0b4a07", "vout": 0}],
        [ {
                "data": "00010203"
            }]
    ],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "createrawtransaction",
    "params": [
        [{"txid": "ee652f0b40209bd02468de0c6336854f5efdd79fb865560aef2c46f4fa0b4a07", "vout": 0}],
        [ {
                "data": "00010203"
            }]
    ],
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
    "method": "createrawtransaction",
    "params": [
        [{"txid": "ee652f0b40209bd02468de0c6336854f5efdd79fb865560aef2c46f4fa0b4a07", "vout": 0}],
        [ {
                "data": "00010203"
            }]
    ],
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
```ruby
use reqwest::header;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = reqwest::Client::new();
    
    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header(header::CONTENT_TYPE, "application/json")
        .body(r#"{
            "jsonrpc": "2.0",
            "method": "createrawtransaction",
            "params": [
                [{"txid": "ee652f0b40209bd02468de0c6336854f5efdd79fb865560aef2c46f4fa0b4a07", "vout": 0}],
                [ {
                "data": "00010203"
            }]
            ],
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
    "result": "0200000001074a0bfaf4462cef0a5665b89fd7fd5e4f8536630cde6824d09b20400b2f65ee0000000000ffffffff01a0860100000000001600149358b74f7cedc75d92f3afb6f47e72e07e2d8f4d00000000"
}
```

### Response Parameters

| Field  | Type   | Description                               |
| ------ | ------ | ----------------------------------------- |
| result | string | The hex-encoded unsigned raw transaction. |

### Use Case

The `createrawtransaction` method is essential for:

* Building custom transactions with precise control
* Creating unsigned transactions for offline signing
* Constructing time-locked transactions
* Building OP\_RETURN data transactions
* Implementing payment batching
* Creating replace-by-fee (RBF) transactions

### Error Handling

| Status Code | Error Message           | Cause                                            |
| ----------- | ----------------------- | ------------------------------------------------ |
| 403         | RBAC: access denied     | Missing or invalid ACCESS-TOKEN.                 |
| -5          | Invalid Bitcoin address | One of the output addresses is invalid.          |
| -3          | Invalid amount          | Amount is out of range or has too many decimals. |

### Integration Notes

The `createrawtransaction` method helps developers:

* Build transactions with complete control over inputs/outputs
* Implement custom UTXO selection algorithms
* Create transactions with embedded data (OP\_RETURN)
* Support legacy signing workflows
* Build advanced payment protocols
