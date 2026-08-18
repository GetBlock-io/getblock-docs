---
description: >-
  Example code for the gettxoutproof JSON-RPC method. Complete guide on how to
  use the gettxoutproof JSON-RPC method in the GetBlock Web3 documentation.
---

# gettxoutproof - Bitcoin Cash

This method returns a hex-encoded proof that one or more transactions were included in a block. The proof can be verified independently with verifytxoutproof.

## Parameters

| Parameter | Type   | Required | Description                                                         |
| --------- | ------ | -------- | ------------------------------------------------------------------- |
| txids     | array  | Yes      | Array of transaction ids to prove                                   |
| blockhash | string | No       | Block hash to look in. Required unless the node maintains a txindex |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "gettxoutproof",
    "params": [["10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642"], "0000000000000000023a561e1ea370153aac5d1504726d1a039032831c05fcfc"],
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
  jsonrpc: '2.0', method: 'gettxoutproof',
  params: [['10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642'], '0000000000000000023a561e1ea370153aac5d1504726d1a039032831c05fcfc'], id: 'getblock.io'
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
        'method': 'gettxoutproof',
        'params': [["10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642"], "0000000000000000023a561e1ea370153aac5d1504726d1a039032831c05fcfc"],
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
            "method": "gettxoutproof",
            "params": [["10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642"], "0000000000000000023a561e1ea370153aac5d1504726d1a039032831c05fcfc"],
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
    "result": "00000020da c2308...4f906497e02830a189e0d6a...proof-hex...00"
}
```

## Response Parameters

| Parameter | Type         | Description                                       |
| --------- | ------------ | ------------------------------------------------- |
| error     | null\|object | Error object when the call fails, otherwise null  |
| id        | string       | Request identifier matching the request           |
| result    | string       | Hex-encoded Merkle proof of transaction inclusion |

## Use Cases

* **SPV Proofs**: Provide inclusion proofs to light clients without full blocks
* **Cross-Chain Relays**: Supply Merkle proofs to a verifying contract on another chain
* **Audit Trails**: Store a verifiable proof that a payment was mined
* **Dispute Resolution**: Demonstrate that a transaction was confirmed in a block

## Error Handling

| Error Code | Message               | Description                                                              |
| ---------- | --------------------- | ------------------------------------------------------------------------ |
| -8         | Invalid parameter     | txid is not a valid 64-character hex string                              |
| -5         | Transaction not found | No transaction with the given id is available; a txindex may be required |
| -32603     | Internal error        | Node failed to read the transaction                                      |
