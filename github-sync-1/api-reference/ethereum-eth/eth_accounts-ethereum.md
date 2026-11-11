---
description: >-
  Example code for the eth_accounts JSON RPC method. Сomplete guide on how to
  use eth_accounts JSON RPC in GetBlock Web3 documentation.
---

# eth\_accounts - Ethereum

This method returns the list of accounts owned by the client. On managed RPC endpoints like GetBlock, this always returns an empty array `[]` — the endpoint doesn't hold user keys. On self-hosted Geth or similar with unlocked wallet accounts, it would return the list of managed addresses.

## Parameters

This method takes no parameters.

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_accounts",
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
    method: 'eth_accounts',
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
        'method': 'eth_accounts',
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
            "method": "eth_accounts",
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
    "result": []
}
```

## Response Parameters

| Parameter | Type            | Description                                                                          |
| --------- | --------------- | ------------------------------------------------------------------------------------ |
| `jsonrpc` | string          | JSON-RPC protocol version ("2.0")                                                    |
| `id`      | string          | Request identifier matching the request                                              |
| `result`  | array of string | Array of account addresses owned by the client — always `[]` on public RPC endpoints |

## Use Cases

* **Client Behavior Detection**: Confirm the endpoint is a public-RPC provider (empty array) rather than a wallet-managing self-hosted node
* **Legacy Wallet Compatibility**: Support older wallet UIs that check `eth_accounts` before initiating a transaction (though modern wallets use `wallet_requestAccounts` via injected providers)
* **Tooling Sanity Checks**: Verify the endpoint responds to standard JSON-RPC method calls

## Error Handling

| Error Code | Message        | Description                           |
| ---------- | -------------- | ------------------------------------- |
| -32603     | Internal error | Node failed to enumerate its accounts |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const accounts = await provider.send('eth_accounts', []);
console.log('Accounts:', accounts);  // [] on public RPC
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { mainnet } from 'viem/chains';

const client = createPublicClient({
    chain: mainnet,
    transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/'),
});

const accounts = await client.request({ method: 'eth_accounts' });
console.log('Accounts:', accounts);
```
{% endcode %}
{% endtab %}
{% endtabs %}
