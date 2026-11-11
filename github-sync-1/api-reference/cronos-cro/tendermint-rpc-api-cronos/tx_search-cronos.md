---
description: >-
  Example code for the tx_search JSON-RPC method. Complete guide on how to use
  tx_search JSON-RPC in GetBlock Web3 documentation.
---

# tx\_search - Cronos

Searches committed transactions by event query, returning matching transactions with their ABCI results and, optionally, Merkle proofs. Results are paginated, so large result sets are read page by page.

## Parameters

| Parameter | Type    | Required | Description                                                  |
| --------- | ------- | -------- | ------------------------------------------------------------ |
| query     | string  | Yes      | CometBFT event query, e.g. `tx.height=12345678`              |
| prove     | boolean | Optional | Include a Merkle proof for each transaction (default: false) |
| page      | string  | Optional | Page number, 1-based (default: 1)                            |
| per\_page | string  | Optional | Results per page (default: 30, max: 100)                     |
| order\_by | string  | Optional | Sort order: `asc` or `desc` (default: `asc`)                 |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "tx_search",
    "params": {
        "query": "tx.height=12345678",
        "prove": false,
        "page": "1",
        "per_page": "30",
        "order_by": "asc"
    }
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
    id: 'getblock.io',
    method: 'tx_search',
    params: {
        "query": "tx.height=12345678",
        "prove": false,
        "page": "1",
        "per_page": "30",
        "order_by": "asc"
    }
}, { headers: { 'Content-Type': 'application/json' } });

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
        'id': 'getblock.io',
        'method': 'tx_search',
        'params': {
            "query": "tx.height=12345678",
            "prove": false,
            "page": "1",
            "per_page": "30",
            "order_by": "asc"
        }
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
        .json(&json!({
            "jsonrpc": "2.0",
            "id": "getblock.io",
            "method": "tx_search",
            "params": {
                "query": "tx.height=12345678",
                "prove": false,
                "page": "1",
                "per_page": "30",
                "order_by": "asc"
            }
        }))
        .send().await?
        .json::<Value>().await?;
    println!("{}", response["result"]);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "txs": [
            {
                "hash": "A1B2C3D4E5F60718293A4B5C6D7E8F90112233445566778899AABBCCDDEEFF00",
                "height": "12345678",
                "index": 0,
                "tx_result": {
                    "code": 0,
                    "gas_wanted": "200000",
                    "gas_used": "118000",
                    "events": []
                },
                "tx": "Cr0BC..."
            }
        ],
        "total_count": "1"
    }
}
```
{% endcode %}

## Response Fields

| Field                   | Type   | Description                                       |
| ----------------------- | ------ | ------------------------------------------------- |
| txs                     | array  | Matching transactions for the requested page      |
| txs\[].hash             | string | Transaction hash                                  |
| txs\[].height           | string | Block height the transaction landed in            |
| txs\[].tx\_result       | object | ABCI result: code, gas, and events                |
| total\_count            | string | Total matching transactions across all pages      |

## Use Cases

* **Address History**: Query transfers with `transfer.recipient='crc1…'`
* **Height Scans**: Fetch every transaction at a height with `tx.height=N`
* **Backfilling**: Page through historical matches with page and per\_page

## Error Handling

| Error                     | Message        | Description                                          |
| ------------------------- | -------------- | ---------------------------------------------------- |
| -32603 / Internal error   | Internal error | The query string is malformed or the page is invalid |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
