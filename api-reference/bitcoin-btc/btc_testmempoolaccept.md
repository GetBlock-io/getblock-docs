---
description: >-
  Example code for the testmempoolaccept JSON-RPC method. Complete guide on how
  to use testmempoolaccept JSON-RPC in GetBlock Web3 documentation.
---

# testmempoolaccept - Bitcoin

This method tests whether raw transactions can be accepted into the mempool without actually submitting them to the network.

### Parameters

| Parameter  | Type          | Required | Description                                                                     |
| ---------- | ------------- | -------- | ------------------------------------------------------------------------------- |
| rawtxs     | array         | Yes      | Array of hex strings of raw transactions.                                       |
| maxfeerate | number/string | No       | Reject transactions whose fee rate is higher than this (default: 0.10 BTC/kvB). |

### Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "testmempoolaccept",
    "params": [["0200000001074a0bfaf4462cef0a5665b89fd7fd5e4f8536630cde6824d09b20400b2f65ee000000006a47304402...00000000"]],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code overflow="wrap" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "testmempoolaccept",
    "params": [["0200000001074a0bfaf4462cef0a5665b89fd7fd5e4f8536630cde6824d09b20400b2f65ee000000006a47304402...00000000"]],
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

{% tab title="Request" %}
{% code overflow="wrap" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
   "method": "testmempoolaccept",
    "params": [["0200000001074a0bfaf4462cef0a5665b89fd7fd5e4f8536630cde6824d09b20400b2f65ee000000006a47304402...00000000"]],
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
{% code overflow="wrap" %}
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
    "params": [["0200000001074a0bfaf4462cef0a5665b89fd7fd5e4f8536630cde6824d09b20400b2f65ee000000006a47304402...00000000"]],
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

{% tabs %}
{% tab title="Accepted" %}
{% code overflow="wrap" %}
```json
{
    "id": "getblock.io",
    "result": [
        {
            "txid": "52273e0ce6cf3452932cfbc1c517c0ce1af1d255fda67a6e3bd63ba1d908c8c2",
            "wtxid": "abc123def456789...",
            "allowed": true,
            "vsize": 141,
            "fees": {
                "base": 0.00001410
            }
        }
    ]
}
```
{% endcode %}
{% endtab %}

{% tab title="Rejected" %}
{% code overflow="wrap" %}
```json
{
    "id": "getblock.io",
    "result": [
        {
            "txid": "52273e0ce6cf3452932cfbc1c517c0ce1af1d255fda67a6e3bd63ba1d908c8c2",
            "allowed": false,
            "reject-reason": "insufficient fee"
        }
    ]
}
```
{% endcode %}
{% endtab %}
{% endtabs %}



### Response Parameters

| Field         | Type    | Description                                 |
| ------------- | ------- | ------------------------------------------- |
| txid          | string  | The transaction hash.                       |
| wtxid         | string  | The witness transaction hash (if accepted). |
| allowed       | boolean | Whether the transaction would be accepted.  |
| vsize         | number  | Virtual transaction size (if accepted).     |
| fees          | object  | Transaction fees (if accepted).             |
| fees.base     | number  | Transaction fee in BTC.                     |
| reject-reason | string  | Rejection reason (if not accepted).         |

### Use Case

The `testmempoolaccept` method is essential for:

* Validating transactions before broadcast
* Testing transaction construction
* Pre-flight checks for payment systems
* Fee validation and adjustment
* Transaction debugging
* Batch transaction validation

### Error Handling

| Status Code | Error Message        | Cause                                              |
| ----------- | -------------------- | -------------------------------------------------- |
| 403         | Forbidden            | Missing or invalid ACCESS-TOKEN.                   |
| -32602      | Invalid params       | Missing or invalid rawtxs array.                   |
| -26         | Transaction rejected | Transaction failed validation (see reject-reason). |
| -22         | TX decode failed     | Invalid raw transaction hex.                       |

#### Common Rejection Reasons

| Reason                         | Description                                   |
| ------------------------------ | --------------------------------------------- |
| insufficient fee               | Fee is below minimum required.                |
| txn-mempool-conflict           | Conflicts with transaction in mempool.        |
| bad-txns-inputs-missingorspent | Input transaction not found or already spent. |
| non-final                      | Transaction is not final (locktime).          |
| absurdly-high-fee              | Fee exceeds maxfeerate.                       |

### Integration With Web3

The `testmempoolaccept` method helps developers:

* Validate transactions before broadcasting
* Build transaction preview features
* Implement fee adequacy checks
* Debug transaction construction issues
* Create safe transaction submission workflows
