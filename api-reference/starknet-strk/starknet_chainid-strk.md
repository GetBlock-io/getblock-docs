# starknet\_chainId - STRK

This method returns the chain ID of the network the node is connected to, encoded as a felt. Mainnet returns SN\_MAIN (0x534e5f4d41494e), used for transaction signing and network guards.

## Parameters

* None

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "starknet_chainId",
    "params": [],
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
    method: 'starknet_chainId',
    params: [],
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
        'method': 'starknet_chainId',
        'params': [],
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
            "method": "starknet_chainId",
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
    "result": "0x534e5f4d41494e"
}
```

## Response Parameters

| Parameter | Type   | Description                                      |
| --------- | ------ | ------------------------------------------------ |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                |
| id        | string | Request identifier matching the request          |
| result    | string | Chain ID as a felt (SN\_MAIN = 0x534e5f4d41494e) |

## Use Cases

* **Transaction Signing**: Include the chain ID in the transaction hash to prevent replay
* **Network Guards**: Reject transactions when the chain ID is unexpected
* **Wallet Onboarding**: Confirm a wallet is pointed at Starknet mainnet
* **Multi-Network Config**: Key configuration off the returned chain ID

## Error Handling

| Error                     | Message          | Description                                       |
| ------------------------- | ---------------- | ------------------------------------------------- |
| 63 / UNEXPECTED\_ERROR    | Unexpected error | The node failed to return the chain ID            |
| 403 / RBAC: access denied | Access denied    | The GetBlock access token is missing or incorrect |
