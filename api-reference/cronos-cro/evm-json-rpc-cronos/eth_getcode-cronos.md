# eth\_getCode - Cronos

This method returns the deployed bytecode at an address at a given block, or 0x for an account with no code. It is used to distinguish contracts from externally owned accounts.

## Parameters

| Parameter      | Type   | Required | Description                                             |
| -------------- | ------ | -------- | ------------------------------------------------------- |
| address        | string | Yes      | 20-byte address to query                                |
| blockParameter | string | Yes      | Block number in hex, or "latest", "earliest", "pending" |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getCode",
    "params": ["0x5C7F8A570d578ED84E63fdFA7b1eE72dEae1AE23", "latest"],
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
    method: 'eth_getCode',
    params: ['0x5C7F8A570d578ED84E63fdFA7b1eE72dEae1AE23', 'latest'],
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
        'method': 'eth_getCode',
        'params': ['0x5C7F8A570d578ED84E63fdFA7b1eE72dEae1AE23', 'latest'],
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
            "method": "eth_getCode",
            "params": ["0x5C7F8A570d578ED84E63fdFA7b1eE72dEae1AE23", "latest"],
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
    "result": "0x60806040523480156100..."
}
```

## Response Parameters

| Parameter | Type   | Description                                  |
| --------- | ------ | -------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")            |
| id        | string | Request identifier matching the request      |
| result    | string | Hex-encoded deployed bytecode, or 0x if none |

## Use Cases

* **Contract Detection**: Check whether an address holds code before calling it
* **Bytecode Verification**: Compare on-chain bytecode against an expected build
* **EIP-7702 Checks**: Detect delegated EOAs that temporarily carry code
* **Deployment Confirmation**: Confirm a contract deployed by reading non-empty code

## Error Handling

| Error Code | Message        | Description                              |
| ---------- | -------------- | ---------------------------------------- |
| -32602     | Invalid params | Malformed address or block parameter     |
| -32603     | Internal error | The node failed to read the account code |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const code = await provider.getCode('0x5C7F8A570d578ED84E63fdFA7b1eE72dEae1AE23');
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';

const client = createPublicClient({
  transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/')
});

const code = await client.getCode({ address: '0x5C7F8A570d578ED84E63fdFA7b1eE72dEae1AE23' });
```
{% endcode %}
{% endtab %}
{% endtabs %}
