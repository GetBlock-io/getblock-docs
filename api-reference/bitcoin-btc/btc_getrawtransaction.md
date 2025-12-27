---
description: >-
  Example code for the getrawtransaction JSON-RPC method. Complete guide on how
  to use getrawtransaction JSON-RPC in GetBlock Web3 documentation.
---

# getrawtransaction - Bitcoin

This method returns the raw transaction data. By default, this function only works for mempool transactions. When called with a blockhash argument, it retrieves the transaction from the specified block.

### Parameters

| Parameter | Type    | Required | Description                                                                |
| --------- | ------- | -------- | -------------------------------------------------------------------------- |
| txid      | string  | Yes      | The transaction id.                                                        |
| verbose   | boolean | No       | If false, return hex string; if true, return JSON object (default: false). |
| blockhash | string  | No       | The block hash to look in for the transaction.                             |

### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getrawtransaction",
    "params": ["52273e0ce6cf3452932cfbc1c517c0ce1af1d255fda67a6e3bd63ba1d908c8c2", true],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
{% code overflow="wrap" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "getrawtransaction",
    "params": ["52273e0ce6cf3452932cfbc1c517c0ce1af1d255fda67a6e3bd63ba1d908c8c2", true],
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
    "method": "getrawtransaction",
    "params": ["52273e0ce6cf3452932cfbc1c517c0ce1af1d255fda67a6e3bd63ba1d908c8c2", True],
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
            "method": "getrawtransaction",
            "params": ["52273e0ce6cf3452932cfbc1c517c0ce1af1d255fda67a6e3bd63ba1d908c8c2", true],
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

### Response (Verbose)

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "txid": "52273e0ce6cf3452932cfbc1c517c0ce1af1d255fda67a6e3bd63ba1d908c8c2",
        "hash": "52273e0ce6cf3452932cfbc1c517c0ce1af1d255fda67a6e3bd63ba1d908c8c2",
        "version": 2,
        "size": 225,
        "vsize": 141,
        "weight": 561,
        "locktime": 0,
        "vin": [
            {
                "txid": "ee652f0b40209bd02468de0c6336854f5efdd79fb865560aef2c46f4fa0b4a07",
                "vout": 0,
                "scriptSig": {
                    "asm": "3044022047ac8e878352d3ebbde1c94ce3a10d057c24175747116f8288e5d794d12d482f02...",
                    "hex": "473044022047ac8e878352d3ebbde1c94ce3a10d057c24175747116f8288e5d794d12d482f02..."
                },
                "txinwitness": [
                    "304402207e8495...01",
                    "02c7b88bdb87..."
                ],
                "sequence": 4294967295
            }
        ],
        "vout": [
            {
                "value": 0.00100000,
                "n": 0,
                "scriptPubKey": {
                    "asm": "0 e8df018c7e326cc253f167d922d5b8e7cd25f1c9",
                    "hex": "0014e8df018c7e326cc253f167d922d5b8e7cd25f1c9",
                    "address": "bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq",
                    "type": "witness_v0_keyhash"
                }
            }
        ],
        "hex": "0200000001074a0bfaf4462cef0a5665b89fd7fd5e...",
        "blockhash": "000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428",
        "confirmations": 15234,
        "time": 1700000000,
        "blocktime": 1700000000
    }
}
```

### Response Parameters

| Field                | Type   | Description                                          |
| -------------------- | ------ | ---------------------------------------------------- |
| txid                 | string | The transaction id.                                  |
| hash                 | string | Transaction hash (differs for witness transactions). |
| version              | number | Transaction version.                                 |
| size                 | number | Serialized transaction size.                         |
| vsize                | number | Virtual transaction size.                            |
| weight               | number | Transaction weight.                                  |
| locktime             | number | Transaction locktime.                                |
| vin                  | array  | Array of transaction inputs.                         |
| vin\[].txid          | string | Previous output transaction id.                      |
| vin\[].vout          | number | Previous output index.                               |
| vin\[].scriptSig     | object | Input script.                                        |
| vin\[].txinwitness   | array  | Witness data.                                        |
| vin\[].sequence      | number | Sequence number.                                     |
| vout                 | array  | Array of transaction outputs.                        |
| vout\[].value        | number | Output value in BTC.                                 |
| vout\[].n            | number | Output index.                                        |
| vout\[].scriptPubKey | object | Output script details.                               |
| hex                  | string | Serialized transaction hex.                          |
| blockhash            | string | Block hash (if confirmed).                           |
| confirmations        | number | Number of confirmations.                             |
| time                 | number | Transaction time.                                    |
| blocktime            | number | Block time.                                          |

### Use Case

The `getrawtransaction` method is essential for:

* Retrieving transaction details
* Building blockchain explorers
* Verifying transaction structure
* Analyzing transaction inputs and outputs
* Extracting witness data
* Supporting wallet transaction lookup

### Error Handling

| Status Code | Error Message               | Cause                                       |
| ----------- | --------------------------- | ------------------------------------------- |
| 403         | Forbidden                   | Missing or invalid ACCESS-TOKEN.            |
| -5          | No such mempool transaction | Transaction not in mempool (use blockhash). |
| -8          | Block not found             | Specified blockhash does not exist.         |

### Integration With Web3

The `getrawtransaction` method helps developers:

* Build transaction explorers
* Create payment verification systems
* Implement transaction history features
* Support UTXO tracking
* Enable detailed transaction analysis
