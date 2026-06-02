---
description: >-
  Example code for the broadcast_tx_async JSON-RPC method. Complete guide on how
  to use broadcast_tx_async JSON-RPC in GetBlock Web3 documentation.
---

# broadcast\_tx\_async - Cosmos

Broadcasts a signed transaction and returns immediately, without running CheckTx synchronously. The fastest broadcast method, but you get no validation feedback — track inclusion via `tx_search` or `subscribe`.

## Parameters

| Parameter | Type   | Required | Description                             |
| --------- | ------ | -------- | --------------------------------------- |
| tx        | string | Yes      | Base64-encoded signed transaction bytes |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
# JSON-RPC over HTTP POST (canonical)
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/broadcast_tx_async' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "broadcast_tx_async",
    "params": {
        "tx": "Co0BCooBChwvY29zbW9zLmJhbmsudjFiZXRhMS5Nc2dTZW5kEmoKLWNvc21vczF0NnJsZ2Y4MGpkOHRhbnBubnZtbXhmcWRjbTljbjd3Y3RyczdoaxItY29zbW9zMWdoZDc1M3NoamVjajRpdjJ4Mmk4eHJrenphbnpqdnZjMjJ4MmtuGgoKBXVhdG9tEgExEnIKWg=="
    },
    "id": "getblock.io"
}'

# URI over HTTP GET (alternate, for simple methods)
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/broadcast_tx_async?tx="Co0BCooBChwvY29zbW9zLmJhbmsudjFiZXRhMS5Nc2dTZW5kEmoKLWNvc21vczF0NnJsZ2Y4MGpkOHRhbnBubnZtbXhmcWRjbTljbjd3Y3RyczdoaxItY29zbW9zMWdoZDc1M3NoamVjajRpdjJ4Mmk4eHJrenphbnpqdnZjMjJ4MmtuGgoKBXVhdG9tEgExEnIKWg=="'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "broadcast_tx_async",
    "params": {
        "tx": "Co0BCooBChwvY29zbW9zLmJhbmsudjFiZXRhMS5Nc2dTZW5kEmoKLWNvc21vczF0NnJsZ2Y4MGpkOHRhbnBubnZtbXhmcWRjbTljbjd3Y3RyczdoaxItY29zbW9zMWdoZDc1M3NoamVjajRpdjJ4Mmk4eHJrenphbnpqdnZjMjJ4MmtuGgoKBXVhdG9tEgExEnIKWg=="
    },
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/broadcast_tx_async',
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

url = "https://go.getblock.io/<ACCESS-TOKEN>/broadcast_tx_async"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "broadcast_tx_async",
    "params": {
        "tx": "Co0BCooBChwvY29zbW9zLmJhbmsudjFiZXRhMS5Nc2dTZW5kEmoKLWNvc21vczF0NnJsZ2Y4MGpkOHRhbnBubnZtbXhmcWRjbTljbjd3Y3RyczdoaxItY29zbW9zMWdoZDc1M3NoamVjajRpdjJ4Mmk4eHJrenphbnpqdnZjMjJ4MmtuGgoKBXVhdG9tEgExEnIKWg=="
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
        "method": "broadcast_tx_async",
        "params": {
                "tx": "Co0BCooBChwvY29zbW9zLmJhbmsudjFiZXRhMS5Nc2dTZW5kEmoKLWNvc21vczF0NnJsZ2Y4MGpkOHRhbnBubnZtbXhmcWRjbTljbjd3Y3RyczdoaxItY29zbW9zMWdoZDc1M3NoamVjajRpdjJ4Mmk4eHJrenphbnpqdnZjMjJ4MmtuGgoKBXVhdG9tEgExEnIKWg=="
        },
        "id": "getblock.io"
    });

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/broadcast_tx_async")
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
        "hash": "B5C8E4F1A6D3B9C2E5F8A1D4B7C0E3F6A9D2B5C8E1F4A7D0B3C6E9F2A5D8B1C4"
    }
}
```
{% endcode %}

## Response Parameters

| Field       | Type   | Description                                                                                            |
| ----------- | ------ | ------------------------------------------------------------------------------------------------------ |
| result.hash | string | Transaction hash — does NOT indicate validity, only that the bytes were accepted by the node for relay |

## Use Cases

* Fire-and-forget submission in high-throughput scenarios
* Bulk transaction injection where individual feedback isn't needed

## Error Handling

| Status Code | Error Message     | Cause                                          |
| ----------- | ----------------- | ---------------------------------------------- |
| 404         | Not Found         | Missing or invalid `<ACCESS-TOKEN>`            |
| -32602      | Invalid params    | Request parameters are missing or malformed    |
| -32603      | Internal error    | Server-side error while processing the request |
| 429         | Too Many Requests | Rate limit exceeded for your plan              |

## SDK Integration

{% tabs %}
{% tab title="CosmJS" %}
```javascript
import { Tendermint37Client, HttpClient } from '@cosmjs/tendermint-rpc';

const httpClient = new HttpClient('https://go.getblock.io/<ACCESS-TOKEN>/');
const client = await Tendermint37Client.create(httpClient);

// CosmJS exposes typed methods on the client (e.g. client.status(), client.block()).
// For raw access to any RPC method:
const result = await httpClient.execute({"jsonrpc": "2.0", "id": "getblock.io", "method": "broadcast_tx_async", "params": {"tx": "Co0BCooBChwvY29zbW9zLmJhbmsudjFiZXRhMS5Nc2dTZW5kEmoKLWNvc21vczF0NnJsZ2Y4MGpkOHRhbnBubnZtbXhmcWRjbTljbjd3Y3RyczdoaxItY29zbW9zMWdoZDc1M3NoamVjajRpdjJ4Mmk4eHJrenphbnpqdnZjMjJ4MmtuGgoKBXVhdG9tEgExEnIKWg=="}});
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
    'method': 'broadcast_tx_async',
    'params': {"tx": "Co0BCooBChwvY29zbW9zLmJhbmsudjFiZXRhMS5Nc2dTZW5kEmoKLWNvc21vczF0NnJsZ2Y4MGpkOHRhbnBubnZtbXhmcWRjbTljbjd3Y3RyczdoaxItY29zbW9zMWdoZDc1M3NoamVjajRpdjJ4Mmk4eHJrenphbnpqdnZjMjJ4MmtuGgoKBXVhdG9tEgExEnIKWg=="}
})
print(response.json())
```
{% endtab %}
{% endtabs %}
