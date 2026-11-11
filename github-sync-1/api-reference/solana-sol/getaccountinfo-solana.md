---
description: >-
  Example code for the getAccountInfo JSON-RPC method. Complete guide on how to
  use getAccountInfo JSON-RPC in GetBlock Web3 documentation.
---

# getAccountInfo - Solana

This method returns the current state of an account, including its lamport balance, owning program, data payload, and rent epoch. This is the primary method for reading on-chain account data such as token mints, PDAs, and program-owned state.

## Parameters

| Parameter | Type   | Required | Description                                              |
| --------- | ------ | -------- | -------------------------------------------------------- |
| pubkey    | string | Yes      | Base58-encoded Pubkey of the account to query            |
| config    | object | No       | Configuration object controlling encoding and commitment |

### Config Object

| Field          | Type   | Required | Description                                                                 |
| -------------- | ------ | -------- | --------------------------------------------------------------------------- |
| encoding       | string | No       | Data encoding: base58, base64, base64+zstd, or jsonParsed                   |
| commitment     | string | No       | Commitment level: processed, confirmed, or finalized. Defaults to finalized |
| dataSlice      | object | No       | Byte range to return, with offset and length fields                         |
| minContextSlot | number | No       | Minimum slot the request can be evaluated at                                |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getAccountInfo",
    "params": ["EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v", {"encoding": "jsonParsed"}],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="@solana/web3.js" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const { Connection, PublicKey } = require('@solana/web3.js');

const connection = new Connection('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', 'confirmed');

const info = await connection.getParsedAccountInfo(
  new PublicKey('EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v')
);

console.log(info.value);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.post(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'getAccountInfo',
        'params': ["EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v", {"encoding": "jsonParsed"}],
        'id': 'getblock.io'
    }
)

print(response.json()['result'])
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="example.rs" overflow="wrap" %}
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
            "method": "getAccountInfo",
            "params": ["EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v", {"encoding": "jsonParsed"}],
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
        "context": {
            "apiVersion": "2.3.6",
            "slot": 397234561
        },
        "value": {
            "data": {
                "parsed": {
                    "info": {
                        "decimals": 6,
                        "freezeAuthority": "7dGbd2QZcCKcTndnHcTL8q7SMVXAkp688NTQYwrRCrar",
                        "isInitialized": true,
                        "mintAuthority": "BJE5MMbqXjVwjAF7oxwPYXnTXDyspzZyt4vwenNw5ruG",
                        "supply": "8721043118453221"
                    },
                    "type": "mint"
                },
                "program": "spl-token",
                "space": 82
            },
            "executable": false,
            "lamports": 389532986339,
            "owner": "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA",
            "rentEpoch": 18446744073709551615,
            "space": 82
        }
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                                  |
| --------- | ------ | ------------------------------------------------------------ |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                            |
| id        | string | Request identifier matching the request                      |
| result    | object | RPC response object containing context and the account value |

### Value Object

| Field      | Type          | Description                                        |
| ---------- | ------------- | -------------------------------------------------- |
| lamports   | number        | Account balance in lamports                        |
| owner      | string        | Base58 Pubkey of the program that owns the account |
| data       | array\|object | Account data, encoded per the requested encoding   |
| executable | boolean       | Whether the account holds a loaded program         |
| rentEpoch  | number        | Epoch at which the account next owes rent          |
| space      | number        | Data length of the account in bytes                |

## Use Cases

* **Token Mint Data**: Read decimals, supply, and mint authority from an SPL Token mint
* **PDA State**: Fetch program-derived account state for Anchor and native programs
* **Account Existence**: Detect whether an associated token account has been initialized
* **Wallet Indexing**: Resolve account owner and executable flags when classifying addresses
* **Partial Reads**: Use dataSlice to fetch a single struct field from a large account

## Error Handling

| Error Code | Message                                   | Description                                                               |
| ---------- | ----------------------------------------- | ------------------------------------------------------------------------- |
| -32602     | Invalid params                            | Malformed base58 Pubkey, unsupported encoding, or invalid dataSlice range |
| -32603     | Internal error                            | Node failed to read the account from its accounts database                |
| -32016     | Minimum context slot has not been reached | The node has not yet processed minContextSlot                             |
| -32005     | Node is unhealthy                         | The node has fallen behind the cluster tip and cannot serve account state |
| -32601     | Method not found                          | Encoding requested is not supported by the node build                     |
