---
description: >-
  Example code for the getblocktemplate JSON-RPC method. Complete guide on how
  to use getblocktemplate JSON-RPC in GetBlock Web3 documentation.
---

# getblocktemplate - Bitcoin

This method returns data needed to construct a block to work on. It is used by miners to get a template for the next block.

### Parameters

| Parameter                      | Type   | Required | Description                                                     |
| ------------------------------ | ------ | -------- | --------------------------------------------------------------- |
| template\_request              | object | No       | Template request parameters.                                    |
| template\_request.mode         | string | No       | Must be "template" or "proposal" (default: "template").         |
| template\_request.capabilities | array  | No       | Array of client capabilities (e.g., "longpoll", "coinbasetxn"). |
| template\_request.rules        | array  | No       | Array of softfork rules to support.                             |

### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getblocktemplate",
    "params": [{"rules": ["segwit"]}],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "getblocktemplate",
    "params": [{"rules": ["segwit"]}],
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
    "method": "getblocktemplate",
    "params": [{"rules": ["segwit"]}],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endtab %}

{% tab title="Ruby" %}
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
            "method": "getblocktemplate",
            "params": [{"rules": ["segwit"]}],
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
        "capabilities": ["proposal"],
        "version": 536870912,
        "rules": ["csv", "segwit", "taproot"],
        "vbavailable": {},
        "vbrequired": 0,
        "previousblockhash": "000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428",
        "transactions": [
            {
                "data": "0200000001...",
                "txid": "abc123...",
                "hash": "def456...",
                "depends": [],
                "fee": 25000,
                "sigops": 4,
                "weight": 900
            }
        ],
        "coinbaseaux": {"flags": ""},
        "coinbasevalue": 312500000,
        "longpollid": "000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab342812345",
        "target": "000000000000000000046b9302e08c16ea186950f42a5498000000000000000",
        "mintime": 1700000000,
        "mutable": ["time", "transactions", "prevblock"],
        "noncerange": "00000000ffffffff",
        "sigoplimit": 80000,
        "sizelimit": 4000000,
        "weightlimit": 4000000,
        "curtime": 1700001000,
        "bits": "170d21b9",
        "height": 820001,
        "default_witness_commitment": "6a24aa21a9ed..."
    }
}
```

### Response Parameters

| Field                        | Type   | Description                                         |
| ---------------------------- | ------ | --------------------------------------------------- |
| version                      | number | Block version.                                      |
| rules                        | array  | Enabled softfork rules.                             |
| previousblockhash            | string | Hash of current tip of the best chain.              |
| transactions                 | array  | Contents of non-coinbase transactions.              |
| coinbaseaux                  | object | Data that should be included in coinbase scriptSig. |
| coinbasevalue                | number | Maximum allowable coinbase value in satoshis.       |
| target                       | string | The hash target.                                    |
| mintime                      | number | Minimum timestamp appropriate for next block.       |
| mutable                      | array  | List of mutable template fields.                    |
| noncerange                   | string | Range of valid nonces.                              |
| sigoplimit                   | number | Limit of sigops per block.                          |
| sizelimit                    | number | Block size limit.                                   |
| weightlimit                  | number | Block weight limit.                                 |
| curtime                      | number | Current timestamp.                                  |
| bits                         | string | Compressed target in compact format.                |
| height                       | number | Height of the next block.                           |
| default\_witness\_commitment | string | Witness commitment for SegWit.                      |

### Use Case

The `getblocktemplate` method is essential for:

* Mining pool software integration
* Solo mining implementations
* Block construction optimization
* Mining software development
* Testing mining configurations
* Understanding block structure

### Error Handling

| Status Code | Error Message              | Cause                               |
| ----------- | -------------------------- | ----------------------------------- |
| 403         | Forbidden                  | Missing or invalid ACCESS-TOKEN.    |
| -9          | Node is downloading blocks | Initial block download in progress. |
| -10         | Node is not connected      | No peer connections available.      |

### Integration Notes

The `getblocktemplate` method helps developers:

* Build mining pool backends
* Create solo mining software
* Implement stratum proxies
* Test block construction logic
* Analyze transaction selection algorithms
