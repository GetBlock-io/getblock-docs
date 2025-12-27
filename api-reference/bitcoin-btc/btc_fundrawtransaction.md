---
description: >-
  Example code for the fundrawtransaction JSON RPC method. Сomplete guide on how
  to use fundrawtransaction JSON RPC in GetBlock Web3 documentation.
---

# fundrawtransaction - Bitcoin {diallowed}

This method adds inputs to a transaction until it has enough value to meet its output value. This will not modify existing inputs, and will add at most one change output to the transaction.

### Parameters

| Parameter                      | Type    | Required | Description                                                      |
| ------------------------------ | ------- | -------- | ---------------------------------------------------------------- |
| hexstring                      | string  | Yes      | The hex string of the raw transaction.                           |
| options                        | object  | No       | Options for funding.                                             |
| options.add\_inputs            | boolean | No       | Include inputs other than watch-only ones (default: true).       |
| options.changeAddress          | string  | No       | The Bitcoin address for receiving change.                        |
| options.changePosition         | number  | No       | The index of the change output.                                  |
| options.conf\_target           | number  | No       | Confirmation target in blocks.                                   |
| options.estimate\_mode         | string  | No       | Fee estimation mode: "unset", "economical", "conservative".      |
| options.fee\_rate              | number  | No       | Fee rate in sat/vB.                                              |
| options.includeWatching        | boolean | No       | Include inputs from watch-only addresses.                        |
| options.lockUnspents           | boolean | No       | Lock selected unspent outputs (default: false).                  |
| options.replaceable            | boolean | No       | Marks transaction as BIP125 replaceable.                         |
| options.subtractFeeFromOutputs | array   | No       | Array of output indexes to subtract fee from.                    |
| iswitness                      | boolean | No       | Whether the transaction hex is a serialized witness transaction. |

### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "fundrawtransaction",
    "params": ["0200000001...00000000", {"conf_target": 6, "replaceable": true}],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "fundrawtransaction",
    "params": ["0200000001...00000000", {"conf_target": 6, "replaceable": true}],
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
    "method": "fundrawtransaction",
    "params": ["0200000001...00000000", {"conf_target": 6, "replaceable": True}],
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
            "method": "fundrawtransaction",
            "params": ["0200000001...00000000", {"conf_target": 6, "replaceable": true}],
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
        "hex": "0200000002074a0bfaf4462cef0a5665b89fd7fd5e...",
        "fee": 0.00001234,
        "changepos": 1
    }
}
```

### Response Parameters

| Field     | Type   | Description                                             |
| --------- | ------ | ------------------------------------------------------- |
| hex       | string | The resulting funded raw transaction hex.               |
| fee       | number | Fee in BTC the transaction will pay.                    |
| changepos | number | The position of the added change output, or -1 if none. |

### Use Case

The `fundrawtransaction` method is essential for:

* Automatically selecting UTXOs for transactions
* Adding change outputs to transactions
* Building transactions with optimal fee rates
* Creating RBF-enabled transactions
* Simplifying transaction construction workflows
* Supporting complex payment scenarios

### Error Handling

| Status Code | Error Message      | Cause                                               |
| ----------- | ------------------ | --------------------------------------------------- |
| 403         | Forbidden          | Missing or invalid ACCESS-TOKEN.                    |
| -4          | Insufficient funds | Not enough funds available to fund the transaction. |
| -22         | TX decode failed   | The transaction hex is malformed.                   |

### Integration Notes

The `fundrawtransaction` method helps developers:

* Build automated UTXO selection
* Create fee-optimal transactions
* Implement change address management
* Support coin control features
* Build batch payment systems
