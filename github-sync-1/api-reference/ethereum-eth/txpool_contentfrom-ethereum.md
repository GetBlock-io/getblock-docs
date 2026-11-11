---
description: >-
  Example code for the txpool_contentFrom JSON RPC method. Complete guide on how
  to use txpool_contentFrom JSON RPC in GetBlock Web3 documentation.
---

# txpool\_contentFrom - Ethereum

This method returns the pending and queued transactions in the node's transaction pool that originate from a single sender address. It is the scoped counterpart to [txpool\_content](txpool_content-ethereum.md), and stays small and fast even when the mempool holds tens of thousands of transactions.

{% hint style="info" %}
Because the sender is already fixed by the parameter, `pending` and `queued` are keyed **directly by nonce**, without the extra address level that `txpool_content` returns.
{% endhint %}

## Parameters

| Parameter | Type   | Required | Description                              |
| --------- | ------ | -------- | ---------------------------------------- |
| `address` | string | Yes      | The sender address to filter the pool by |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "txpool_contentFrom",
    "params": ["0x000025e01DB606436e2A658C765CcB78442b1c69"],
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
    method: 'txpool_contentFrom',
    params: ['0x000025e01DB606436e2A658C765CcB78442b1c69'],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

const { pending, queued } = response.data.result;
console.log('Pending nonces:', Object.keys(pending));
console.log('Queued nonces:', Object.keys(queued));
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
        'method': 'txpool_contentFrom',
        'params': ['0x000025e01DB606436e2A658C765CcB78442b1c69'],
        'id': 'getblock.io'
    }
)

result = response.json()['result']
print(f"Pending nonces: {list(result['pending'])}")
print(f"Queued nonces: {list(result['queued'])}")
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
            "method": "txpool_contentFrom",
            "params": ["0x000025e01DB606436e2A658C765CcB78442b1c69"],
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
        "pending": {
            "0": {
                "blockHash": null,
                "blockNumber": null,
                "blockTimestamp": null,
                "from": "0x000025e01db606436e2a658c765ccb78442b1c69",
                "gas": "0x59d8",
                "gasPrice": "0x99cf00",
                "hash": "0x49460eb9e2eb98b7d404f12815601246f2e9033d2bf2efa0655dd45ba3b64af9",
                "input": "0xa9059cbb000000000000000000000000b0ed81f27a195bc81ce3e063c28a3adc669c26140000000000000000000000000000000000000000000000000000000000989680",
                "nonce": "0x0",
                "to": "0xdac17f958d2ee523a2206206994597c13d831ec7",
                "transactionIndex": null,
                "value": "0x0",
                "type": "0x0",
                "chainId": "0x1",
                "v": "0x25",
                "r": "0x9b01f005aafbc6ff1123d7801a291e496d7ad775c15cfbb488bb3a2946cf44c7",
                "s": "0x7c754b63c87923305b37f7f35321ace3d49be8138fe8269d967fb34a1c0d5e2f"
            }
        },
        "queued": {}
    }
}
```

When the address has nothing in the pool, both objects come back empty:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "pending": {},
        "queued": {}
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                                                  |
| --------- | ------ | ---------------------------------------------------------------------------- |
| `jsonrpc` | string | JSON-RPC protocol version ("2.0")                                            |
| `id`      | string | Request identifier matching the request                                      |
| `pending` | object | Transactions from the address eligible for inclusion, keyed by decimal nonce |
| `queued`  | object | Transactions from the address not yet processable, keyed by decimal nonce    |

Each transaction object carries the same fields as [txpool\_content](txpool_content-ethereum.md): `from`, `to`, `gas`, `gasPrice`, `nonce`, `value`, `input`, `type`, `chainId`, and the `v` / `r` / `s` signature components, with `blockHash`, `blockNumber`, `blockTimestamp`, and `transactionIndex` set to `null` while the transaction is still pooled.

## Use Cases

* **Transaction Tracking**: Check whether a transaction you submitted is still sitting in the pool
* **Nonce Gap Detection**: Identify the missing nonce that is blocking an account's queued transactions
* **Hot Wallet Monitoring**: Watch one sending account's outstanding transactions without pulling the whole mempool
* **Replacement Confirmation**: Verify a fee-bumped transaction has displaced the original at the same nonce

## Error Handling

| Error Code | Message                               | Description                                         |
| ---------- | ------------------------------------- | --------------------------------------------------- |
| -32601     | Method not found                      | The `txpool` module is not enabled on this endpoint |
| -32602     | missing value for required argument 0 | The address parameter was omitted                   |
| -32603     | Internal error                        | Node failed to read the transaction pool            |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const address = '0x000025e01DB606436e2A658C765CcB78442b1c69';
const content = await provider.send('txpool_contentFrom', [address]);
console.log('Pending nonces:', Object.keys(content.pending));
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

const content = await client.request({
    method: 'txpool_contentFrom',
    params: ['0x000025e01DB606436e2A658C765CcB78442b1c69'],
});
console.log('Pending nonces:', Object.keys(content.pending));
```
{% endcode %}
{% endtab %}
{% endtabs %}
