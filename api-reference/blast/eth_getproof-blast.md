# eth\_getProof - Blast

This method returns the account and storage Merkle-Patricia proofs for an address at a given block. It is used to verify account state against a block's state root without trusting the node.

## Parameters

| Parameter      | Type   | Required | Description                                             |
| -------------- | ------ | -------- | ------------------------------------------------------- |
| address        | string | Yes      | 20-byte account address                                 |
| storageKeys    | array  | Yes      | Array of 32-byte storage slot keys to prove             |
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
    "method": "eth_getProof",
    "params": ["0x4200000000000000000000000000000000000006", ["0x0000000000000000000000000000000000000000000000000000000000000000"], "latest"],
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
    method: 'eth_getProof',
    params: ['0x4200000000000000000000000000000000000006', ['0x0000000000000000000000000000000000000000000000000000000000000000'], 'latest'],
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
        'method': 'eth_getProof',
        'params': ['0x4200000000000000000000000000000000000006', ['0x0000000000000000000000000000000000000000000000000000000000000000'], 'latest'],
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
            "method": "eth_getProof",
            "params": ["0x4200000000000000000000000000000000000006", ["0x0000000000000000000000000000000000000000000000000000000000000000"], "latest"],
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
        "address": "0x4200000000000000000000000000000000000006",
        "balance": "0x0",
        "codeHash": "0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470",
        "nonce": "0x1",
        "storageHash": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
        "accountProof": [
            "0xf90211a0..."
        ],
        "storageProof": [
            {
                "key": "0x0000000000000000000000000000000000000000000000000000000000000000",
                "value": "0x0",
                "proof": [
                    "0xf90211a0..."
                ]
            }
        ]
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                                       |
| --------- | ------ | ----------------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                                 |
| id        | string | Request identifier matching the request                           |
| result    | object | Account fields plus account and storage proofs (see fields below) |

### Result Object

| Field        | Type   | Description                                           |
| ------------ | ------ | ----------------------------------------------------- |
| balance      | string | Account balance in wei (hex)                          |
| codeHash     | string | Keccak-256 hash of the account code                   |
| nonce        | string | Account nonce (hex)                                   |
| storageHash  | string | Root hash of the account storage trie                 |
| accountProof | array  | RLP-encoded Merkle-Patricia nodes proving the account |
| storageProof | array  | Per-key proofs with key, value, and node list         |

## Use Cases

* **Trustless Verification**: Verify account state against a known state root
* **Light Clients**: Prove balances and storage without full state
* **Cross-Chain Bridges**: Feed storage proofs into a verifier contract
* **Audit Evidence**: Produce a cryptographic proof of a slot value at a block

## Error Handling

| Error Code | Message        | Description                                             |
| ---------- | -------------- | ------------------------------------------------------- |
| -32602     | Invalid params | Malformed address, storage key list, or block parameter |
| -32603     | Internal error | The node failed to build the proof                      |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const proof = await provider.send('eth_getProof', ['0x4200000000000000000000000000000000000006', ['0x00...00'], 'latest']);
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

const proof = await client.getProof({ address: '0x4200000000000000000000000000000000000006', storageKeys: ['0x00...00'] });
```
{% endcode %}
{% endtab %}
{% endtabs %}
