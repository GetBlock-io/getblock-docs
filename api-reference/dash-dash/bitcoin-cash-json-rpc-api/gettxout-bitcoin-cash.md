---
description: >-
  Example code for the gettxout JSON-RPC method. Complete guide on how to use
  the gettxout JSON-RPC method in the GetBlock Web3 documentation.
---

# gettxout - Bitcoin Cash

This method returns details about an unspent transaction output. It is the primary method for checking whether a specific output is still spendable.

## Parameters

| Parameter        | Type    | Required | Description                                  |
| ---------------- | ------- | -------- | -------------------------------------------- |
| txid             | string  | Yes      | The transaction id                           |
| n                | numeric | Yes      | The output index (vout)                      |
| include\_mempool | boolean | No       | Whether to include the mempool. Default true |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "gettxout",
    "params": ["10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642", 1, true],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="bitcoinjs-lib" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const { data } = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
  jsonrpc: '2.0', method: 'gettxout',
  params: ['10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642', 1, true], id: 'getblock.io'
});
console.log(data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests

response = requests.post(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'gettxout',
        'params': ["10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642", 1, true],
        'id': 'getblock.io'
    }
)

print(response.json()['result'])
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="example.rs" %}
```rust
use reqwest::Client;
use serde_json::{json, Value};

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let response = client
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "gettxout",
            "params": ["10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642", 1, true],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;

    println!("Result: {}", response["result"]);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "bestblockhash": "0000000000000000023a561e1ea370153aac5d1504726d1a039032831c05fcfc",
        "confirmations": 1197,
        "value": 0.00105345,
        "scriptPubKey": {
            "asm": "OP_HASH160 f5b48d1130dc3d366d1eabf6783a552d1c8e08f4 OP_EQUAL",
            "hex": "a914f5b48d1130dc3d366d1eabf6783a552d1c8e08f487",
            "reqSigs": 1,
            "type": "scripthash",
            "addresses": [
                "bitcoincash:ppm2qsznhks23z7629mms6s4cwef74vcwvn0h829pq"
            ]
        },
        "coinbase": false
    }
}
```

## Response Parameters

| Parameter | Type         | Description                                                              |
| --------- | ------------ | ------------------------------------------------------------------------ |
| error     | null\|object | Error object when the call fails, otherwise null                         |
| id        | string       | Request identifier matching the request                                  |
| result    | object\|null | Output details, or null if the output is unspent set does not contain it |

### Result Object

| Field         | Type    | Description                                            |
| ------------- | ------- | ------------------------------------------------------ |
| bestblockhash | string  | Hash of the block at the chain tip                     |
| confirmations | numeric | Confirmations of the transaction containing the output |
| value         | numeric | Output value in BCH                                    |
| scriptPubKey  | object  | The output script, including type and addresses        |
| coinbase      | boolean | Whether the output is from a coinbase transaction      |

## Use Cases

* **UTXO Validation**: Confirm an output is unspent before building a spend
* **Balance Construction**: Sum spendable outputs to compute an address balance
* **Double-Spend Checks**: Detect that an output has already been consumed
* **Coin Selection**: Read output values when choosing inputs for a transaction

## Error Handling

| Error Code | Message               | Description                                                              |
| ---------- | --------------------- | ------------------------------------------------------------------------ |
| -8         | Invalid parameter     | txid is not a valid 64-character hex string                              |
| -5         | Transaction not found | No transaction with the given id is available; a txindex may be required |
| -32603     | Internal error        | Node failed to read the transaction                                      |
