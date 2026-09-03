---
description: >-
  Example code for the starknet_estimateMessageFee JSON-RPC method. Complete
  guide on how to use starknet_estimateMessageFee JSON-RPC in GetBlock Web3
  documentation.
---

# starknet\_estimateMessageFee - STRK

This method estimates the fee for a message sent from Ethereum L1 to a Starknet L2 contract, at a given block. It is used when bridging from L1.

## Parameters

| Parameter | Type   | Required | Description                                                           |
| --------- | ------ | -------- | --------------------------------------------------------------------- |
| message   | object | Yes      | from\_address (L1), to\_address (L2), entry\_point\_selector, payload |
| block\_id | object | string   | Yes                                                                   |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "starknet_estimateMessageFee",
    "params": [{ "from_address": "0xbe1259ff905cadbbaa62514388b71bdefb8aacc1", "to_address": "0x049d36570d4e46f48e99674bd3fcc84644ddd6b96f7c741b1562b82f9e004dc7", "entry_point_selector": "0x2e4263afad30923c891518314c3c95dbe830a16874e8abc5777a9a20b54c76e", "payload": ["0x1", "0x2"] }, "latest"],
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
    method: 'starknet_estimateMessageFee',
    params: [{ from_address: '0xbe1259ff905cadbbaa62514388b71bdefb8aacc1', to_address: '0x049d36570d4e46f48e99674bd3fcc84644ddd6b96f7c741b1562b82f9e004dc7', entry_point_selector: '0x2e4263afad30923c891518314c3c95dbe830a16874e8abc5777a9a20b54c76e', payload: ['0x1', '0x2'] }, 'latest'],
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
        'method': 'starknet_estimateMessageFee',
        'params': [{ 'from_address': '0xbe1259ff905cadbbaa62514388b71bdefb8aacc1', 'to_address': '0x049d36570d4e46f48e99674bd3fcc84644ddd6b96f7c741b1562b82f9e004dc7', 'entry_point_selector': '0x2e4263afad30923c891518314c3c95dbe830a16874e8abc5777a9a20b54c76e', 'payload': ['0x1', '0x2'] }, 'latest'],
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
            "method": "starknet_estimateMessageFee",
            "params": [{ "from_address": "0xbe1259ff905cadbbaa62514388b71bdefb8aacc1", "to_address": "0x049d36570d4e46f48e99674bd3fcc84644ddd6b96f7c741b1562b82f9e004dc7", "entry_point_selector": "0x2e4263afad30923c891518314c3c95dbe830a16874e8abc5777a9a20b54c76e", "payload": ["0x1", "0x2"] }, "latest"],
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
    "result": {
        "l1_gas_consumed": "0x1a4",
        "l1_gas_price": "0x3b9aca00",
        "l2_gas_consumed": "0x0",
        "l2_gas_price": "0x0",
        "overall_fee": "0x1ff973cafa7fff",
        "unit": "WEI"
    }
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| id        | string | Request identifier matching the request |
| result    | object | Fee estimate for the L1-to-L2 message   |

## Use Cases

* **Bridge UX**: Show the L1-to-L2 message fee before bridging
* **Cost Planning**: Budget message fees for cross-layer flows
* **Integration Testing**: Validate message parameters produce a fee
* **Fee Monitoring**: Track message costs over time

## Error Handling

| Error                              | Message                     | Description                                    |
| ---------------------------------- | --------------------------- | ---------------------------------------------- |
| 41 / TRANSACTION\_EXECUTION\_ERROR | Transaction execution error | The message handler reverted during estimation |
| 20 / CONTRACT\_NOT\_FOUND          | Contract not found          | The L2 target contract does not exist          |
