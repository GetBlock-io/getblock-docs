---
description: >-
  Example code for the eth_getLogs JSON_RPC method. Complete guide on how to use
  eth_getLogs JSON_RPC method in GetBlock Web3 documentation.
---

# eth\_getLogs - Tron

This method returns event logs matching a filter over a range of blocks, in the Ethereum-compatible shape. It is used to read TRC-20 events such as transfers.

## Parameters

| Parameter | Type   | Required | Description                                |
| --------- | ------ | -------- | ------------------------------------------ |
| filter    | object | Yes      | Filter object selecting the logs to return |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/jsonrpc' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getLogs",
    "params": [{"fromBlock": "0x40d2900", "toBlock": "0x40d2a00", "address": "0x41a614f803b6fd780986a42c78ec9c7f77e6ded13c", "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]}],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/jsonrpc', {
    jsonrpc: '2.0',
    method: 'eth_getLogs',
    params: [{"fromBlock": "0x40d2900", "toBlock": "0x40d2a00", "address": "0x41a614f803b6fd780986a42c78ec9c7f77e6ded13c", "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]}],
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/jsonrpc',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'eth_getLogs',
        'params': [{"fromBlock": "0x40d2900", "toBlock": "0x40d2a00", "address": "0x41a614f803b6fd780986a42c78ec9c7f77e6ded13c", "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]}],
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
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/jsonrpc")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "eth_getLogs",
            "params": [{"fromBlock": "0x40d2900", "toBlock": "0x40d2a00", "address": "0x41a614f803b6fd780986a42c78ec9c7f77e6ded13c", "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]}],
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
            "address": "0x41a614f803b6fd780986a42c78ec9c7f77e6ded13c",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                "0x000000000000000000000000f0cc5a2a84cd0f68ed1667070934542d673acbd8"
            ],
            "data": "0x00000000000000000000000000000000000000000000000000000000000f4240",
            "blockNumber": "0x40d2980",
            "transactionHash": "0xd5ec749ecc2a615399d8a6c864ea4c74ff9f523c2be0e341ac9be5d47d7c2d62",
            "logIndex": "0x0"
        }
    ]
}
```

## Response Parameters

| Parameter | Type   | Description                              |
| --------- | ------ | ---------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")        |
| id        | string | Request identifier matching the request  |
| result    | array  | Array of log objects matching the filter |

### Log Object

| Field           | Type   | Description                                             |
| --------------- | ------ | ------------------------------------------------------- |
| address         | string | Contract that emitted the log, in hex form              |
| topics          | array  | Indexed event topics, starting with the event signature |
| data            | string | Non-indexed event data, hex-encoded                     |
| transactionHash | string | Transaction that emitted the log                        |

## Use Cases

* **TRC-20 Transfers**: Read token Transfer events for an address or token
* **Event Indexing**: Ingest contract events into a database
* **Monitoring**: Trigger alerts on specific contract events
* **Analytics**: Aggregate historical events over block ranges

## Error Handling

| Error Code | Message        | Description                                            |
| ---------- | -------------- | ------------------------------------------------------ |
| -32602     | Invalid params | A parameter is missing or has the wrong type or format |
| -32603     | Internal error | The node failed to process the request                 |
