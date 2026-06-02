---
description: >-
  Example code for the check_tx JSON-RPC method. Complete guide on how to use
  check_tx JSON-RPC in GetBlock Web3 documentation.
---

# check\_tx - Cosmos

Runs CheckTx against the application without broadcasting the transaction to the network. Use this to validate a signed transaction before deciding to broadcast — checks signature, sequence, fees, etc.

## Parameters

| Parameter | Type   | Required | Description                             |
| --------- | ------ | -------- | --------------------------------------- |
| tx        | string | Yes      | Base64-encoded signed transaction bytes |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
# JSON-RPC over HTTP POST (canonical)
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/check_tx' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "check_tx",
    "params": {
        "tx": "Co0BCooBChwvY29zbW9zLmJhbmsudjFiZXRhMS5Nc2dTZW5kEmoKLWNvc21vczF0NnJsZ2Y4MGpkOHRhbnBubnZtbXhmcWRjbTljbjd3Y3RyczdoaxItY29zbW9zMWdoZDc1M3NoamVjajRpdjJ4Mmk4eHJrenphbnpqdnZjMjJ4MmtuGgoKBXVhdG9tEgExEnIKWg=="
    },
    "id": "getblock.io"
}'

# URI over HTTP GET (alternate, for simple methods)
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/check_tx?tx="Co0BCooBChwvY29zbW9zLmJhbmsudjFiZXRhMS5Nc2dTZW5kEmoKLWNvc21vczF0NnJsZ2Y4MGpkOHRhbnBubnZtbXhmcWRjbTljbjd3Y3RyczdoaxItY29zbW9zMWdoZDc1M3NoamVjajRpdjJ4Mmk4eHJrenphbnpqdnZjMjJ4MmtuGgoKBXVhdG9tEgExEnIKWg=="'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "check_tx",
    "params": {
        "tx": "Co0BCooBChwvY29zbW9zLmJhbmsudjFiZXRhMS5Nc2dTZW5kEmoKLWNvc21vczF0NnJsZ2Y4MGpkOHRhbnBubnZtbXhmcWRjbTljbjd3Y3RyczdoaxItY29zbW9zMWdoZDc1M3NoamVjajRpdjJ4Mmk4eHJrenphbnpqdnZjMjJ4MmtuGgoKBXVhdG9tEgExEnIKWg=="
    },
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/check_tx',
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

url = "https://go.getblock.io/<ACCESS-TOKEN>/check_tx"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "check_tx",
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
        "method": "check_tx",
        "params": {
                "tx": "Co0BCooBChwvY29zbW9zLmJhbmsudjFiZXRhMS5Nc2dTZW5kEmoKLWNvc21vczF0NnJsZ2Y4MGpkOHRhbnBubnZtbXhmcWRjbTljbjd3Y3RyczdoaxItY29zbW9zMWdoZDc1M3NoamVjajRpdjJ4Mmk4eHJrenphbnpqdnZjMjJ4MmtuGgoKBXVhdG9tEgExEnIKWg=="
        },
        "id": "getblock.io"
});

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/check_tx")
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
        "code": 0,
        "data": "",
        "log": "",
        "gas_wanted": "200000",
        "gas_used": "0",
        "events": [],
        "codespace": ""
    }
}
```
{% endcode %}

## Response Parameters

| Field              | Type    | Description                              |
| ------------------ | ------- | ---------------------------------------- |
| result.code        | integer | `0` if the transaction would be accepted |
| result.log         | string  | Failure reason if `code` is non-zero     |
| result.gas\_wanted | string  | Gas limit declared by the transaction    |
| result.codespace   | string  | Module codespace for non-zero codes      |

## Use Cases

* Pre-flight transaction validation in wallets
* Simulating transactions for fee estimation
* Debugging signing failures before broadcast

## Error Handling

| Status Code | Error Message     | Cause                                          |
| ----------- | ----------------- | ---------------------------------------------- |
| 404         | Not found         | Missing or invalid `<ACCESS-TOKEN>`            |
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
const result = await httpClient.execute({"jsonrpc": "2.0", "id": "getblock.io", "method": "check_tx", "params": {"tx": "Co0BCooBChwvY29zbW9zLmJhbmsudjFiZXRhMS5Nc2dTZW5kEmoKLWNvc21vczF0NnJsZ2Y4MGpkOHRhbnBubnZtbXhmcWRjbTljbjd3Y3RyczdoaxItY29zbW9zMWdoZDc1M3NoamVjajRpdjJ4Mmk4eHJrenphbnpqdnZjMjJ4MmtuGgoKBXVhdG9tEgExEnIKWg=="}});
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
    'method': 'check_tx',
    'params': {"tx": "Co0BCooBChwvY29zbW9zLmJhbmsudjFiZXRhMS5Nc2dTZW5kEmoKLWNvc21vczF0NnJsZ2Y4MGpkOHRhbnBubnZtbXhmcWRjbTljbjd3Y3RyczdoaxItY29zbW9zMWdoZDc1M3NoamVjajRpdjJ4Mmk4eHJrenphbnpqdnZjMjJ4MmtuGgoKBXVhdG9tEgExEnIKWg=="}
}})
print(response.json())
```
{% endtab %}
{% endtabs %}
