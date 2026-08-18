---
description: >-
  Example code for the getGenesisHash JSON-RPC method. Complete guide on how to
  use the getGenesisHash JSON-RPC method in the GetBlock Web3 documentation.
---

# getGenesisHash - Solana

This method returns the genesis hash of the connected cluster. The value uniquely identifies Mainnet Beta, Devnet, Testnet, and any local cluster.

## Parameters

This method does not accept any parameters.

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getGenesisHash",
    "params": [],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="@solana/web3.js" %}
{% code title="example.js" %}
```javascript
const { Connection } = require('@solana/web3.js');

const connection = new Connection('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', 'confirmed');

const genesisHash = await connection.getGenesisHash();

console.log(genesisHash);
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
        'method': 'getGenesisHash',
        'params': [],
        'id': 'getblock.io'
    }
)

print(response.json()['result'])
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
            "method": "getGenesisHash",
            "params": [],
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
    "result": "5eykt4UsFv8P8NJdTREpY1vzqKqZKvdpKuc147dw2N9d"
}
```

## Response Parameters

| Parameter | Type   | Description                                          |
| --------- | ------ | ---------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                    |
| id        | string | Request identifier matching the request              |
| result    | string | Base58-encoded genesis hash of the connected cluster |

## Use Cases

* **Cluster Assertion**: Fail fast when an endpoint points at the wrong cluster
* **Config Validation**: Verify a deployment script targets Mainnet Beta before broadcasting
* **Wallet Safety**: Warn a user when a dApp requests a signature on an unexpected cluster
* **Test Isolation**: Confirm CI runs against a fresh local validator rather than Devnet

## Error Handling

| Error Code | Message           | Description                                     |
| ---------- | ----------------- | ----------------------------------------------- |
| -32603     | Internal error    | Node failed to read the requested cluster state |
| -32005     | Node is unhealthy | The node has fallen behind the cluster tip      |
