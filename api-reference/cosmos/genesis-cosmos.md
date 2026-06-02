---
description: >-
  Example code for the genesis JSON-RPC method. Complete guide on how to use
  genesis JSON-RPC in GetBlock Web3 documentation.
---

# genesis - Cosmos

Returns the genesis document for the chain.&#x20;

{% hint style="info" %}
Cosmos Hub's genesis file is too large for this method — it returns an error. Use `genesis_chunked` instead for any large chain.
{% endhint %}

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
# JSON-RPC over HTTP POST (canonical)
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "genesis",
    "params": [],
    "id": "getblock.io"
}'

# URI over HTTP GET (alternate, for simple methods)
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/genesis'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "genesis",
    "params": [],
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
    "method": "genesis",
    "params": [],
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
        "method": "genesis",
        "params": [],
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
        "genesis": {
            "genesis_time": "2019-12-11T16:11:34Z",
            "chain_id": "cosmoshub-4",
            "initial_height": "5200791",
            "consensus_params": {},
            "app_hash": "...",
            "app_state": {}
        }
    }
}
```
{% endcode %}

## Response Parameters

| Field                          | Type   | Description                                                                      |
| ------------------------------ | ------ | -------------------------------------------------------------------------------- |
| result.genesis.genesis\_time   | string | ISO 8601 timestamp at genesis                                                    |
| result.genesis.chain\_id       | string | Chain ID at genesis                                                              |
| result.genesis.initial\_height | string | Initial block height (used after a chain upgrade like cosmoshub-1 → cosmoshub-4) |
| result.genesis.app\_state      | object | Initial application state — typically very large on production chains            |

## Use Cases

* Genesis inspection on small / test chains
* Initial state verification for new chain launches

## Error Handling

| Status Code | Error Message          | Cause                                                          |
| ----------- | ---------------------- | -------------------------------------------------------------- |
| 404         | Not found              | Missing or invalid `<ACCESS-TOKEN>`                            |
| -32602      | Invalid params         | Request parameters are missing or malformed                    |
| -32603      | Internal error         | Server-side error while processing the request                 |
| 429         | Too Many Requests      | Rate limit exceeded for your plan                              |
| -32603      | Genesis file too large | Cosmos Hub's genesis is >100MB — use `genesis_chunked` instead |

## SDK Integration

{% tabs %}
{% tab title="CosmJS" %}
```javascript
import { Tendermint37Client, HttpClient } from '@cosmjs/tendermint-rpc';

const httpClient = new HttpClient('https://go.getblock.io/<ACCESS-TOKEN>/');
const client = await Tendermint37Client.create(httpClient);

// CosmJS exposes typed methods on the client (e.g. client.status(), client.block()).
// For raw access to any RPC method:
const result = await httpClient.execute({"jsonrpc": "2.0", "id": "getblock.io", "method": "genesis", "params": []});
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
    'method': 'genesis',
    'params': []
}})
print(response.json())
```
{% endtab %}
{% endtabs %}
