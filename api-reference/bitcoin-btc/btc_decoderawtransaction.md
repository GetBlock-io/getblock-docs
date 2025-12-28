---
description: >-
  Example code for the decoderawtransaction JSON RPC method. Сomplete guide on
  how to use decoderawtransactionJSON RPC in GetBlock Web3 documentation.
---

# decoderawtransaction - Bitcoin

This method decodes a hex-encoded raw transaction and returns a JSON object with all transaction details.

### Parameters

| Parameter | Type    | Required | Description                                                      |
| --------- | ------- | -------- | ---------------------------------------------------------------- |
| hexstring | string  | Yes      | The hex-encoded raw transaction string.                          |
| iswitness | boolean | No       | Whether the transaction hex is a witness serialized transaction. |

### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "decoderawtransaction",
    "params": ["0200000001074a0bfaf4462cef0a5665b89fd7fd5e4f8536630cde6824d09b20400b2f65ee0000000000ffffffff01a0860100000000001600149358b74f7cedc75d92f3afb6f47e72e07e2d8f4d00000000"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "decoderawtransaction",
    "params": ["0200000001074a0bfaf4462cef0a5665b89fd7fd5e4f8536630cde6824d09b20400b2f65ee0000000000ffffffff01a0860100000000001600149358b74f7cedc75d92f3afb6f47e72e07e2d8f4d00000000"],
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
    "method": "decoderawtransaction",
    "params": ["0200000001074a0bfaf4462cef0a5665b89fd7fd5e4f8536630cde6824d09b20400b2f65ee0000000000ffffffff01a0860100000000001600149358b74f7cedc75d92f3afb6f47e72e07e2d8f4d00000000"],
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
            "method": "decoderawtransaction",
            "params": ["0200000001074a0bfaf4462cef0a5665b89fd7fd5e4f8536630cde6824d09b20400b2f65ee0000000000ffffffff01a0860100000000001600149358b74f7cedc75d92f3afb6f47e72e07e2d8f4d00000000"],
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
    "result": {
        "txid": "054183ee699ff12cf1c9f4413f421c3cf0a8febc6b82a1d8751a30f5e0fd54c0",
        "hash": "b07f26a908955f8071620896476d6e2ca6b6610e5edc9e55f19185ca9536c8de",
        "version": 2,
        "size": 222,
        "vsize": 141,
        "weight": 561,
        "locktime": 894415,
        "vin": [
            {
                "txid": "b9795d456bd0cca5ce90c2621d98f9559a0f728e971c82fe75e924c407bdd642",
                "vout": 0,
                "scriptSig": {
                    "asm": "",
                    "hex": ""
                },
                "txinwitness": [
                    "304402204549b10ea1698d4b4605be1d3bfff810f6135395386207664a66f5bb8bd1470902205fb86c95bfbe7b502195ffa65276d292fb074fe18ba171ef0a3539521138409101",
                    "03cb5de30b343b35cc09b50f064eb289dd3e252f6985fc4309fba795cc950316af"
                ],
                "sequence": 4294967293
            }
        ],
        "vout": [
            {
                "value": 0.10000000,
                "n": 0,
                "scriptPubKey": {
                    "asm": "0 d3620fe180a0beb8fcc9682fe92e5a25f0feccd3",
                    "desc": "addr(bc1q6d3qlcvq5zlt3lxfdqh7jtj6yhc0anxn80rw96)#wu542cq0",
                    "hex": "0014d3620fe180a0beb8fcc9682fe92e5a25f0feccd3",
                    "address": "bc1q6d3qlcvq5zlt3lxfdqh7jtj6yhc0anxn80rw96",
                    "type": "witness_v0_keyhash"
                }
            },
            {
                "value": 0.19910000,
                "n": 1,
                "scriptPubKey": {
                    "asm": "0 076d998f6b12c3d1da6217d967154f42a5e0afd3",
                    "desc": "addr(bc1qqakenrmtztparknzzlvkw920g2j7pt7n4kx0sa)#jr6axfjt",
                    "hex": "0014076d998f6b12c3d1da6217d967154f42a5e0afd3",
                    "address": "bc1qqakenrmtztparknzzlvkw920g2j7pt7n4kx0sa",
                    "type": "witness_v0_keyhash"
                }
            }
        ]
    },
    "id": "getblock.io"
}
```

### Response Parameters

| Field                | Type   | Description                                               |
| -------------------- | ------ | --------------------------------------------------------- |
| txid                 | string | The transaction id (hash of the transaction).             |
| hash                 | string | The transaction hash (differs from txid for witness txs). |
| version              | number | The transaction version.                                  |
| size                 | number | The serialized transaction size in bytes.                 |
| vsize                | number | The virtual transaction size in vbytes.                   |
| weight               | number | The transaction weight (used for fee calculation).        |
| locktime             | number | The transaction locktime.                                 |
| vin                  | array  | Array of transaction inputs.                              |
| vin\[].txid          | string | The txid of the previous output.                          |
| vin\[].vout          | number | The output index of the previous output.                  |
| vin\[].scriptSig     | object | The input script.                                         |
| vin\[].sequence      | number | The input sequence number.                                |
| vout                 | array  | Array of transaction outputs.                             |
| vout\[].value        | number | The output value in BTC.                                  |
| vout\[].n            | number | The output index.                                         |
| vout\[].scriptPubKey | object | The output script details.                                |

### Use Case

The `decoderawtransaction` method is essential for:

* Inspecting unsigned transactions before signing
* Verifying transaction structure and outputs
* Debugging transaction construction
* Building transaction explorers
* Validating transactions received from external sources
* Analyzing transaction formats

### Error Handling

| Status Code | Error Message    | Cause                                        |
| ----------- | ---------------- | -------------------------------------------- |
| 403         | Forbidden        | Missing or invalid ACCESS-TOKEN.             |
| -22         | TX decode failed | The transaction hex is malformed or invalid. |

### Integration Notes

The `decoderawtransaction` method helps developers:

* Build transaction verification interfaces
* Implement pre-signing transaction review
* Create blockchain explorers
* Debug transaction construction
* Support transparent wallet operations
