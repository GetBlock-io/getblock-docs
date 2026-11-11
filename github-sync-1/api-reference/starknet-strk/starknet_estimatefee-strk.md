---
description: >-
  Example code for the starknet_estimateFee JSON-RPC method. Complete guide on
  how to use starknet_estimateFee JSON-RPC in GetBlock Web3 documentation.
---

# starknet\_estimateFee - STRK

This method estimates the fee for one or more transactions without executing them on chain, at a given block. It returns gas consumption and overall fee per transaction.

## Parameters

| Parameter         | Type             | Required | Description                                |
| ----------------- | ---------------- | -------- | ------------------------------------------ |
| request           | array            | Yes      | Array of transactions to estimate          |
| simulation\_flags | array            | Yes      | Flags such as SKIP\_VALIDATE               |
| block\_id         | object \| string | Yes      | {block\_number} or {block\_hash}, or a tag |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "starknet_estimateFee",
    "params": [[{ "type": "INVOKE", "version": "0x1", "sender_address": "0x07c57808b9cea7130c44aab2f8ca6147b04408943b48c6d8c3c83eb8cfdd8c0b", "nonce": "0x2b", "max_fee": "0x0", "signature": [], "calldata": ["0x1", "0x049d36570d4e46f48e99674bd3fcc84644ddd6b96f7c741b1562b82f9e004dc7", "0x2e4263afad30923c891518314c3c95dbe830a16874e8abc5777a9a20b54c76e", "0x1", "0x07c57808b9cea7130c44aab2f8ca6147b04408943b48c6d8c3c83eb8cfdd8c0b"] }], [], "latest"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'starknet_estimateFee',
    params: [[{ type: 'INVOKE', version: '0x1', sender_address: '0x07c57808b9cea7130c44aab2f8ca6147b04408943b48c6d8c3c83eb8cfdd8c0b', nonce: '0x2b', max_fee: '0x0', signature: [], calldata: ['0x1', '0x049d36570d4e46f48e99674bd3fcc84644ddd6b96f7c741b1562b82f9e004dc7', '0x2e4263afad30923c891518314c3c95dbe830a16874e8abc5777a9a20b54c76e', '0x1', '0x07c57808b9cea7130c44aab2f8ca6147b04408943b48c6d8c3c83eb8cfdd8c0b'] }], [], 'latest'],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

console.log(response.data.result);
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
        'method': 'starknet_estimateFee',
        'params': [[{ 'type': 'INVOKE', 'version': '0x1', 'sender_address': '0x07c57808b9cea7130c44aab2f8ca6147b04408943b48c6d8c3c83eb8cfdd8c0b', 'nonce': '0x2b', 'max_fee': '0x0', 'signature': [], 'calldata': ['0x1', '0x049d36570d4e46f48e99674bd3fcc84644ddd6b96f7c741b1562b82f9e004dc7', '0x2e4263afad30923c891518314c3c95dbe830a16874e8abc5777a9a20b54c76e', '0x1', '0x07c57808b9cea7130c44aab2f8ca6147b04408943b48c6d8c3c83eb8cfdd8c0b'] }], [], 'latest'],
        'id': 'getblock.io'
    }
)

print(response.json())
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
            "method": "starknet_estimateFee",
            "params": [[{ "type": "INVOKE", "version": "0x1", "sender_address": "0x07c57808b9cea7130c44aab2f8ca6147b04408943b48c6d8c3c83eb8cfdd8c0b", "nonce": "0x2b", "max_fee": "0x0", "signature": [], "calldata": ["0x1", "0x049d36570d4e46f48e99674bd3fcc84644ddd6b96f7c741b1562b82f9e004dc7", "0x2e4263afad30923c891518314c3c95dbe830a16874e8abc5777a9a20b54c76e", "0x1", "0x07c57808b9cea7130c44aab2f8ca6147b04408943b48c6d8c3c83eb8cfdd8c0b"] }], [], "latest"],
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
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": [
        {
            "l1_gas_consumed": "0x1a4",
            "l1_gas_price": "0x3b9aca00",
            "l2_gas_consumed": "0x0",
            "l2_gas_price": "0x0",
            "overall_fee": "0x1ff973cafa7fff",
            "unit": "WEI"
        }
    ]
}
```

## Response Parameters

| Parameter | Type   | Description                                      |
| --------- | ------ | ------------------------------------------------ |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                |
| id        | string | Request identifier matching the request          |
| result    | array  | Per-transaction fee estimates (see fields below) |

### Result Object

| Field             | Type   | Description                          |
| ----------------- | ------ | ------------------------------------ |
| overall\_fee      | string | Estimated total fee as a felt        |
| l1\_gas\_consumed | string | Estimated L1 gas used                |
| unit              | string | Fee unit (WEI for ETH, FRI for STRK) |

## Use Cases

* **Fee Preview**: Show an estimated fee before a user signs
* **max\_fee Sizing**: Set a safe max\_fee from the estimate
* **Batch Costing**: Estimate several transactions at once
* **Validation Skips**: Use SKIP\_VALIDATE to estimate unsigned transactions

## Error Handling

| Error                              | Message                     | Description                              |
| ---------------------------------- | --------------------------- | ---------------------------------------- |
| 41 / TRANSACTION\_EXECUTION\_ERROR | Transaction execution error | A transaction reverted during estimation |
| 24 / BLOCK\_NOT\_FOUND             | Block not found             | No block matches the supplied block\_id  |
