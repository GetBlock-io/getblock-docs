---
description: >-
  Example code for the chain_getFinalizedHead JSON-RPC method. Complete guide on
  how to use chain_getFinalizedHead JSON-RPC in GetBlock Web3 documentation.
---

# chain\_getFinalizedHead - Midnight

This method returns the hash of the latest finalized block. Finalized blocks are confirmed by GRANDPA and cannot be reverted. The spelling variant `chain_getFinalisedHead` is also supported and returns the same value.

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "chain_getFinalizedHead",
    "params": [],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "chain_getFinalizedHead",
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
    "method": "chain_getFinalizedHead",
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
        "method": "chain_getFinalizedHead",
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

```json
{
    "jsonrpc": "2.0",
    "result": "0xb26f48fdafcedfd991a681e9c0e910e60c4d7beac3c9b18d4ff834e82d63b3d8",
    "id": "getblock.io"
}
```

## Response Parameters

| Field  | Type   | Description                        |
| ------ | ------ | ---------------------------------- |
| result | string | Hash of the latest finalized block |

## Use Cases

* Finality-aware indexers that only process irreversible blocks
* Computing safe confirmation depth
* Bridges and exchanges that wait for finality before crediting deposits

## Error Handling

| Status Code | Error Message     | Cause                                                         |
| ----------- | ----------------- | ------------------------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                           |
| -32602      | Invalid params    | Request parameters are missing or malformed                   |
| -32601      | Method not found  | The method is not enabled on this node or has been deprecated |
| 429         | Too Many Requests | Rate limit exceeded for your plan                             |

## SDK Integration

{% tabs %}
{% tab title="@polkadot/api (JavaScript)" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const wsProvider = new WsProvider('wss://go.getblock.io/<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider: wsProvider });

// Call via the typed API:
const result = await api.rpc.chain.getFinalizedHead();
console.log(result.toJSON());

// Or call the raw JSON-RPC method:
const raw = await api.rpc.rpc.methods();
// const raw = await (api as any)._rpcCore.provider.send('chain_getFinalizedHead', []);
```
{% endtab %}

{% tab title="substrate-interface (Python)" %}
```python
from substrateinterface import SubstrateInterface

substrate = SubstrateInterface(url='wss://go.getblock.io/<ACCESS-TOKEN>/')

# Generic RPC call:
result = substrate.rpc_request('chain_getFinalizedHead', [])
print(result)
```
{% endtab %}
{% endtabs %}
