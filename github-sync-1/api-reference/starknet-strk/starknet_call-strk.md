---
description: >-
  Example code for the starknet_call JSON-RPC method. Complete guide on how to
  use starknet_call JSON-RPC in GetBlock Web3 documentation.
---

# starknet\_call - STRK

This method executes a read-only function on a contract at a given block and returns its result as an array of felts. No transaction is created and no fee is charged.

## Parameters

| Parameter | Type             | Required | Description                                                       |
| --------- | ---------------- | -------- | ----------------------------------------------------------------- |
| request   | object           | Yes      | Call details: contract\_address, entry\_point\_selector, calldata |
| block\_id | object \| string | Yes      | {block\_number} or {block\_hash}, or a tag                        |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "starknet_call",
    "params": [{ "contract_address": "0x049d36570d4e46f48e99674bd3fcc84644ddd6b96f7c741b1562b82f9e004dc7", "entry_point_selector": "0x2e4263afad30923c891518314c3c95dbe830a16874e8abc5777a9a20b54c76e", "calldata": ["0x07c57808b9cea7130c44aab2f8ca6147b04408943b48c6d8c3c83eb8cfdd8c0b"] }, "latest"],
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
    method: 'starknet_call',
    params: [{ contract_address: '0x049d36570d4e46f48e99674bd3fcc84644ddd6b96f7c741b1562b82f9e004dc7', entry_point_selector: '0x2e4263afad30923c891518314c3c95dbe830a16874e8abc5777a9a20b54c76e', calldata: ['0x07c57808b9cea7130c44aab2f8ca6147b04408943b48c6d8c3c83eb8cfdd8c0b'] }, 'latest'],
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
        'method': 'starknet_call',
        'params': [{ 'contract_address': '0x049d36570d4e46f48e99674bd3fcc84644ddd6b96f7c741b1562b82f9e004dc7', 'entry_point_selector': '0x2e4263afad30923c891518314c3c95dbe830a16874e8abc5777a9a20b54c76e', 'calldata': ['0x07c57808b9cea7130c44aab2f8ca6147b04408943b48c6d8c3c83eb8cfdd8c0b'] }, 'latest'],
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
            "method": "starknet_call",
            "params": [{ "contract_address": "0x049d36570d4e46f48e99674bd3fcc84644ddd6b96f7c741b1562b82f9e004dc7", "entry_point_selector": "0x2e4263afad30923c891518314c3c95dbe830a16874e8abc5777a9a20b54c76e", "calldata": ["0x07c57808b9cea7130c44aab2f8ca6147b04408943b48c6d8c3c83eb8cfdd8c0b"] }, "latest"],
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
        "0x2386f26fc10000",
        "0x0"
    ]
}
```

## Response Parameters

| Parameter | Type   | Description                                   |
| --------- | ------ | --------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")             |
| id        | string | Request identifier matching the request       |
| result    | array  | Return values of the called function as felts |

## Use Cases

* **Token Balances**: Call balanceOf on ERC-20 style contracts
* **Contract Reads**: Invoke any view function without a transaction
* **Price Feeds**: Read on-chain view data for dashboards
* **dApp Backends**: Fetch contract state via typed calls

## Error Handling

| Error                     | Message            | Description                                      |
| ------------------------- | ------------------ | ------------------------------------------------ |
| 40 / CONTRACT\_ERROR      | Contract error     | The call reverted or the selector does not exist |
| 20 / CONTRACT\_NOT\_FOUND | Contract not found | No contract exists at the address at that block  |
