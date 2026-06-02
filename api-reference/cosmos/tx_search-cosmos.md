---
description: >-
  Example code for the tx_search JSON-RPC method. Complete guide on how to use
  tx_search JSON-RPC in GetBlock Web3 documentation.
---

# tx\_search - Cosmos

Searches transactions matching a CometBFT event query. Queries use the Tendermint event query syntax (e.g. `transfer.recipient='cosmos1...'`). Paginated — use `page` and `per_page` to traverse large result sets.

## Parameters

| Parameter | Type    | Required | Description                                        |
| --------- | ------- | -------- | -------------------------------------------------- |
| query     | string  | Yes      | Event query string (see CometBFT query syntax)     |
| prove     | boolean | No       | Include Merkle proofs (default: false)             |
| page      | integer | No       | Page number (default: 1)                           |
| per\_page | integer | No       | Results per page (default: 30, max: 100)           |
| order\_by | string  | No       | Sort order: `"asc"` or `"desc"` (default: `"asc"`) |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
# JSON-RPC over HTTP POST (canonical)
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "tx_search",
    "params": {
        "query": "transfer.recipient='cosmos1t6rlgf80jd8tanpnnvmmxfqdcm9cn7wctrs7hk'",
        "prove": false,
        "page": 1,
        "per_page": 30,
        "order_by": "desc"
    },
    "id": "getblock.io"
}'

# URI over HTTP GET (alternate, for simple methods)
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/tx_search?query="transfer.recipient='cosmos1t6rlgf80jd8tanpnnvmmxfqdcm9cn7wctrs7hk'"&prove=false&page=1&per_page=30&order_by="desc"'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "tx_search",
    "params": {
        "query": "transfer.recipient='cosmos1t6rlgf80jd8tanpnnvmmxfqdcm9cn7wctrs7hk'",
        "prove": false,
        "page": 1,
        "per_page": 30,
        "order_by": "desc"
    },
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
    .then(response => console.log(JSON.stringify(response.data, null, 2)))
    .catch(error => console.log(error));
```
{% endtab %}

{% tab title="Python (Requests)" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "tx_search",
    "params": {
        "query": "transfer.recipient='cosmos1t6rlgf80jd8tanpnnvmmxfqdcm9cn7wctrs7hk'",
        "prove": false,
        "page": 1,
        "per_page": 30,
        "order_by": "desc"
    },
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endtab %}

{% tab title="Rust (Reqwest)" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let payload = json!({
        "jsonrpc": "2.0",
        "method": "tx_search",
        "params": {
                "query": "transfer.recipient='cosmos1t6rlgf80jd8tanpnnvmmxfqdcm9cn7wctrs7hk'",
                "prove": false,
                "page": 1,
                "per_page": 30,
                "order_by": "desc"
        },
        "id": "getblock.io"
});

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&payload)
        .send()
        .await?;

    let result: serde_json::Value = response.json().await?;
    println!("{:#?}", result);

    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "txs": [
            {
                "hash": "B5C8E4F1A6D3B9C2E5F8A1D4B7C0E3F6A9D2B5C8E1F4A7D0B3C6E9F2A5D8B1C4",
                "height": "23456789",
                "index": 0,
                "tx_result": {
                    "code": 0,
                    "gas_used": "67890",
                    "events": []
                },
                "tx": "Co0BCooBChwvY29zbW9zLmJhbmsudjFiZXRhMS5Nc2dTZW5kEmoKLWNvc21vczF0NnJsZ2Y4MGpkOHRhbnBubnZtbXhmcWRjbTljbjd3Y3RyczdoaxItY29zbW9zMWdoZDc1M3NoamVjajRpdjJ4Mmk4eHJrenphbnpqdnZjMjJ4MmtuGgoKBXVhdG9tEgExEnIKWg=="
            }
        ],
        "total_count": "1"
    }
}
```
{% endcode %}

## Response Parameters

| Field                           | Type   | Description                                  |
| ------------------------------- | ------ | -------------------------------------------- |
| result.txs                      | array  | Matching transactions                        |
| result.txs\[].hash              | string | Transaction hash                             |
| result.txs\[].height            | string | Block height                                 |
| result.txs\[].tx\_result.events | array  | Events emitted by the transaction            |
| result.total\_count             | string | Total matching transactions across all pages |

## Use Cases

* Account activity history (queries like `transfer.recipient='cosmos1...'`)
* Staking event tracking (`delegate.validator='cosmosvaloper1...'`)
* IBC packet history (`recv_packet.packet_src_channel='channel-N'`)
* Gov proposal vote auditing (`proposal_vote.proposal_id='123'`)

## Error Handling

| Status Code | Error Message     | Cause                                                                 |
| ----------- | ----------------- | --------------------------------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                                   |
| -32602      | Invalid params    | Request parameters are missing or malformed                           |
| -32603      | Internal error    | Server-side error while processing the request                        |
| 429         | Too Many Requests | Rate limit exceeded for your plan                                     |
| -32602      | Invalid query     | The event query string is malformed or references unknown event types |

## SDK Integration

{% tabs %}
{% tab title="CosmJS" %}
```javascript
import { Tendermint37Client, HttpClient } from '@cosmjs/tendermint-rpc';

const httpClient = new HttpClient('https://go.getblock.io/<ACCESS-TOKEN>/');
const client = await Tendermint37Client.create(httpClient);

// CosmJS exposes typed methods on the client (e.g. client.status(), client.block()).
// For raw access to any RPC method:
const result = await httpClient.execute({"jsonrpc": "2.0", "id": "getblock.io", "method": "tx_search", "params": {"query": "transfer.recipient='cosmos1t6rlgf80jd8tanpnnvmmxfqdcm9cn7wctrs7hk'", "prove": false, "page": 1, "per_page": 30, "order_by": "desc"}});
console.log(result);
```
{% endtab %}

{% tab title="cosmpy / requests" %}
```python
# cosmpy wraps Cosmos REST / gRPC. For raw CometBFT RPC access, use requests.
import requests

response = requests.post('https://go.getblock.io/<ACCESS-TOKEN>/', json={
    'jsonrpc': '2.0',
    'id': 'getblock.io',
    'method': 'tx_search',
    'params': {"query": "transfer.recipient='cosmos1t6rlgf80jd8tanpnnvmmxfqdcm9cn7wctrs7hk'", "prove": false, "page": 1, "per_page": 30, "order_by": "desc"}
}})
print(response.json())
```
{% endtab %}
{% endtabs %}
