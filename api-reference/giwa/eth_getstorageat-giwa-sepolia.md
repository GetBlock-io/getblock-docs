# eth\_getStorageAt - GIWA Sepolia

This method returns the 32-byte value stored at a given storage slot of a contract at a specific block. It is used to read raw contract state that has no getter.

## Parameters

| Parameter      | Type   | Required | Description                                             |
| -------------- | ------ | -------- | ------------------------------------------------------- |
| address        | string | Yes      | 20-byte contract address                                |
| position       | string | Yes      | Storage slot index in hex                               |
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
    "method": "eth_getStorageAt",
    "params": ["0x4200000000000000000000000000000000000006", "0x0", "latest"],
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
    method: 'eth_getStorageAt',
    params: ['0x4200000000000000000000000000000000000006', '0x0', 'latest'],
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
        'method': 'eth_getStorageAt',
        'params': ['0x4200000000000000000000000000000000000006', '0x0', 'latest'],
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
            "method": "eth_getStorageAt",
            "params": ["0x4200000000000000000000000000000000000006", "0x0", "latest"],
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
    "result": "0x0000000000000000000000000000000000000000000000000000000000000000"
}
```

## Response Parameters

| Parameter | Type   | Description                                 |
| --------- | ------ | ------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")           |
| id        | string | Request identifier matching the request     |
| result    | string | 32-byte value at the requested storage slot |

## Use Cases

* **Raw State Reads**: Inspect a slot that a contract does not expose via a getter
* **Proxy Inspection**: Read an implementation address from a known proxy slot
* **Mapping Access**: Read a mapping entry by computing its slot hash
* **Audit & Forensics**: Verify on-chain state during incident analysis

## Error Handling

| Error Code | Message        | Description                                       |
| ---------- | -------------- | ------------------------------------------------- |
| -32602     | Invalid params | Malformed address, slot index, or block parameter |
| -32603     | Internal error | The node failed to read the storage slot          |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const value = await provider.getStorage('0x4200000000000000000000000000000000000006', 0);
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

const value = await client.getStorageAt({ address: '0x4200000000000000000000000000000000000006', slot: '0x0' });
```
{% endcode %}
{% endtab %}
{% endtabs %}
