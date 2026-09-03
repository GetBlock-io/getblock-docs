---
description: >-
  Example code for the eth_createAccessList JSON-RPC method. Complete guide on
  how to use eth_createAccessList JSON-RPC in GetBlock Web3 documentation.
---

# eth\_createAccessList - Blast

This method returns an EIP-2930 access list for a transaction along with the gas used, which can reduce gas costs for transactions that touch many storage slots.

## Parameters

| Parameter      | Type   | Required | Description                                 |
| -------------- | ------ | -------- | ------------------------------------------- |
| transaction    | object | Yes      | Transaction call object                     |
| blockParameter | string | No       | Block number in hex, or "latest", "pending" |

### Transaction Object

| Field | Type   | Required | Description                        |
| ----- | ------ | -------- | ---------------------------------- |
| from  | string | No       | 20-byte sender address             |
| to    | string | Yes      | 20-byte recipient/contract address |
| data  | string | No       | Encoded function call data         |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_createAccessList",
    "params": [{ "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "to": "0x4200000000000000000000000000000000000006", "data": "0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045" }, "latest"],
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
    method: 'eth_createAccessList',
    params: [{ from: '0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045', to: '0x4200000000000000000000000000000000000006', data: '0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045' }, 'latest'],
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
        'method': 'eth_createAccessList',
        'params': [{ 'from': '0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045', 'to': '0x4200000000000000000000000000000000000006', 'data': '0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045' }, 'latest'],
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
            "method": "eth_createAccessList",
            "params": [{ "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "to": "0x4200000000000000000000000000000000000006", "data": "0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045" }, "latest"],
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
    "result": {
        "accessList": [
            {
                "address": "0x4200000000000000000000000000000000000006",
                "storageKeys": [
                    "0x0000000000000000000000000000000000000000000000000000000000000000"
                ]
            }
        ],
        "gasUsed": "0x64a3"
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                  |
| --------- | ------ | -------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")            |
| id        | string | Request identifier matching the request      |
| result    | object | Access list plus gas used (see fields below) |

### Result Object

| Field      | Type   | Description                                         |
| ---------- | ------ | --------------------------------------------------- |
| accessList | array  | Addresses and storage keys the transaction accesses |
| gasUsed    | string | Gas used when the access list is applied (hex)      |

## Use Cases

* **Gas Optimization**: Attach an access list to reduce SLOAD costs on hot slots
* **Type-1 Transactions**: Build EIP-2930 access-list transactions
* **Simulation**: Discover which storage slots a call touches
* **Tooling**: Feed generated access lists into transaction builders

## Error Handling

| Error Code | Message         | Description                                           |
| ---------- | --------------- | ----------------------------------------------------- |
| -32602     | Invalid params  | Malformed transaction object or block parameter       |
| -32000     | Execution error | The transaction failed while building the access list |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const al = await provider.send('eth_createAccessList', [{ from: '0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045', to: '0x4200000000000000000000000000000000000006', data: '0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045' }, 'latest']);
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

const al = await client.request({ method: 'eth_createAccessList', params: [{ from: '0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045', to: '0x4200000000000000000000000000000000000006', data: '0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045' }, 'latest'] });
```
{% endcode %}
{% endtab %}
{% endtabs %}
