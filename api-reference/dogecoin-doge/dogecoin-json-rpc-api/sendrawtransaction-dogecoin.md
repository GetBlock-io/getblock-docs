---
description: >-
  Example code for the sendrawtransaction JSON-RPC method. Complete guide on how to use
  sendrawtransaction JSON-RPC in GetBlock Web3 documentation.
---

# sendrawtransaction - Dogecoin

This method submits a signed, serialized transaction to the network and returns its transaction id on acceptance. The transaction must already be fully signed.

## Parameters

| Parameter       | Type    | Required | Description                                                          |
| --------------- | ------- | -------- | ---------------------------------------------------------------------- |
| hexstring       | string  | Yes      | The hex-encoded signed raw transaction                               |
| allowhighfees   | boolean | No       | Allow a fee above the node's safety threshold. Default false         |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "sendrawtransaction",
    "params": ["0100000001...", false],
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
  jsonrpc: '2.0', method: 'sendrawtransaction', params: ["0100000001...", false], id: 'getblock.io'
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
        'method': 'sendrawtransaction',
        'params': ["0100000001...", false],
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
            "method": "sendrawtransaction",
            "params": ["0100000001...", false],
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
    "result": "d0dd61c93cd4a571030c15b5f6a94466a8f7246b986dd318d656c910e6e5a8ed"
}
```

## Response Parameters

| Parameter | Type         | Description                                      |
| --------- | ------------ | ------------------------------------------------ |
| error     | null\|object | Error object when the call fails, otherwise null |
| id        | string       | Request identifier matching the request          |
| result | string | Transaction id of the accepted transaction |

{% hint style="warning" %}
Acceptance means the node admitted the transaction to its mempool, not that it has been mined. Track it to the required depth before treating a payment as settled.

Dogecoin's minimum relay fee is `0.01` DOGE per kilobyte, far above Bitcoin's default. A fee sized from a Bitcoin-derived constant is rejected with error `-26`. Size the fee from the Blockbook [estimatefee](../dogecoin-blockbook-rest-api/api-v2-estimatefee-dogecoin.md) endpoint, and note that a one-block target there returns `-1` rather than a usable rate.
{% endhint %}

## Use Cases

* **Wallet Sends**: Broadcast a transaction built and signed on the client
* **Payment Settlement**: Push a payout to the network
* **Consolidation**: Broadcast a transaction combining several small outputs
* **Retry Flows**: Rebroadcast a transaction that has not been mined

## Error Handling

| Error Code | Message | Description |
| ---------- | ------- | ----------- |
| -22 | TX decode failed | The hex is malformed or not a complete transaction |
| -25 | Missing inputs | An input does not exist or is already spent |
| -26 | Transaction rejected | The transaction failed a policy or consensus check, such as the minimum relay fee |
| -27 | Transaction already in block chain | The transaction has already been mined |
