---
description: >-
  Example code for the gettxout JSON-RPC method. Complete guide on how to use
  gettxout JSON-RPC in GetBlock Web3 documentation.
---

# gettxout - Bitcoin

This method returns details about an unspent transaction output (UTXO).

### Parameters

| Parameter        | Type    | Required | Description                                     |
| ---------------- | ------- | -------- | ----------------------------------------------- |
| txid             | string  | Yes      | The transaction id.                             |
| n                | number  | Yes      | The vout index.                                 |
| include\_mempool | boolean | No       | Whether to include the mempool (default: true). |

### Request

{% tabs %}
{% tab title="curl" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "gettxout",
    "params": ["52273e0ce6cf3452932cfbc1c517c0ce1af1d255fda67a6e3bd63ba1d908c8c2", 0, true],
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
    "method": "gettxout",
    "params": ["52273e0ce6cf3452932cfbc1c517c0ce1af1d255fda67a6e3bd63ba1d908c8c2", 0, true],
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
    "method": "gettxout",
    "params": ["52273e0ce6cf3452932cfbc1c517c0ce1af1d255fda67a6e3bd63ba1d908c8c2", 0, True],
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
            "method": "gettxout",
            "params": ["52273e0ce6cf3452932cfbc1c517c0ce1af1d255fda67a6e3bd63ba1d908c8c2", 0, true],
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
        "bestblock": "000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428",
        "confirmations": 15234,
        "value": 0.00100000,
        "scriptPubKey": {
            "asm": "0 e8df018c7e326cc253f167d922d5b8e7cd25f1c9",
            "hex": "0014e8df018c7e326cc253f167d922d5b8e7cd25f1c9",
            "type": "witness_v0_keyhash",
            "address": "bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq"
        },
        "coinbase": false
    }
}
```

### Response Parameters

| Field                | Type    | Description                                    |
| -------------------- | ------- | ---------------------------------------------- |
| bestblock            | string  | Block hash at height of best block.            |
| confirmations        | number  | Number of confirmations.                       |
| value                | number  | Transaction output value in BTC.               |
| scriptPubKey         | object  | The output script.                             |
| scriptPubKey.asm     | string  | Script in assembly format.                     |
| scriptPubKey.hex     | string  | Script in hex format.                          |
| scriptPubKey.type    | string  | Script type (e.g., witness\_v0\_keyhash).      |
| scriptPubKey.address | string  | Bitcoin address (if applicable).               |
| coinbase             | boolean | Whether this is a coinbase transaction output. |

### Use Case

The `gettxout` method is essential for:

* Checking if a UTXO is still unspent
* Validating transaction inputs before signing
* Building UTXO-based wallet systems
* Verifying payment completion
* Implementing double-spend detection
* Supporting coin selection algorithms

### Error Handling

| Status Code | Error Message  | Cause                            |
| ----------- | -------------- | -------------------------------- |
| 403         | Forbidden      | Missing or invalid ACCESS-TOKEN. |
| -8          | Invalid params | Missing txid or vout parameter.  |

{% hint style="info" %}
Returns `null` if the output is not found or has been spent.
{% endhint %}

### Integration With Web3&#x20;

The `gettxout` method helps developers:

* Build UTXO management systems
* Implement transaction validation
* Create payment confirmation tools
* Support coin selection algorithms
* Enable balance verification
