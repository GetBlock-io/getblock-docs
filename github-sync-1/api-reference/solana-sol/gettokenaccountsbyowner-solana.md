---
description: >-
  Example code for the getTokenAccountsByOwner JSON-RPC method. Complete guide
  on how to use the getTokenAccountsByOwner JSON-RPC method in the GetBlock Web3
  documentation.
---

# getTokenAccountsByOwner - Solana

This method returns all SPL Token accounts held by an owner address. Results can be narrowed to a single mint or a single token program, and returned in parsed form.

## Parameters

| Parameter | Type   | Required | Description                                              |
| --------- | ------ | -------- | -------------------------------------------------------- |
| owner     | string | Yes      | Base58-encoded Pubkey of the token account owner         |
| filter    | object | Yes      | Filter object specifying either a mint or a programId    |
| config    | object | No       | Configuration object controlling encoding and commitment |

### Filter Object

| Field     | Type   | Required | Description                                              |
| --------- | ------ | -------- | -------------------------------------------------------- |
| mint      | string | No       | Restrict results to token accounts for this mint         |
| programId | string | No       | Restrict results to accounts owned by this token program |

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
    "method": "getTokenAccountsByOwner",
    "params": ["JUP6LkbZbjS1jKKwapdHNy74zcZ3tLUZoi5QNyVTaV4", {"programId": "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA"}, {"encoding": "jsonParsed"}],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="@solana/web3.js" %}
{% code title="example.js" %}
```javascript
const { Connection, PublicKey } = require('@solana/web3.js');

const connection = new Connection('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', 'confirmed');

const { value } = await connection.getParsedTokenAccountsByOwner(
  new PublicKey('JUP6LkbZbjS1jKKwapdHNy74zcZ3tLUZoi5QNyVTaV4'),
  { programId: new PublicKey('TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA') }
);

console.log(value.length);
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
        'method': 'getTokenAccountsByOwner',
        'params': ["JUP6LkbZbjS1jKKwapdHNy74zcZ3tLUZoi5QNyVTaV4", {"programId": "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA"}, {"encoding": "jsonParsed"}],
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
            "method": "getTokenAccountsByOwner",
            "params": ["JUP6LkbZbjS1jKKwapdHNy74zcZ3tLUZoi5QNyVTaV4", {"programId": "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA"}, {"encoding": "jsonParsed"}],
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
        "value": [
            {
                "account": {
                    "data": {
                        "parsed": {
                            "info": {
                                "isNative": false,
                                "mint": "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v",
                                "owner": "JUP6LkbZbjS1jKKwapdHNy74zcZ3tLUZoi5QNyVTaV4",
                                "state": "initialized",
                                "tokenAmount": {
                                    "amount": "1250000000",
                                    "decimals": 6,
                                    "uiAmount": 1250.0,
                                    "uiAmountString": "1250"
                                }
                            },
                            "type": "account"
                        },
                        "program": "spl-token",
                        "space": 165
                    },
                    "executable": false,
                    "lamports": 2039280,
                    "owner": "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA",
                    "rentEpoch": 18446744073709551615,
                    "space": 165
                },
                "pubkey": "3emsAVdmGKERbHjmGfQ6oZ1e35dkf5z6TmFYcCQnpB2M"
            }
        ]
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                                                     |
| --------- | ------ | ------------------------------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                                               |
| id        | string | Request identifier matching the request                                         |
| result    | object | RPC response object where value is an array of token accounts held by the owner |

### Value Entry

| Field   | Type   | Description                                                         |
| ------- | ------ | ------------------------------------------------------------------- |
| pubkey  | string | Base58 Pubkey of the token account                                  |
| account | object | Account object containing the parsed or encoded token account state |

## Use Cases

* **Portfolio Rendering**: List every token a wallet holds in one request
* **ATA Discovery**: Locate the associated token account for a mint and owner pair
* **Token-2022 Support**: Query the Token-2022 program separately from the legacy program
* **Balance Aggregation**: Sum holdings across multiple token accounts for the same mint
* **Dust Cleanup**: Identify zero-balance accounts whose rent can be reclaimed

## Error Handling

| Error Code | Message                                   | Description                                                            |
| ---------- | ----------------------------------------- | ---------------------------------------------------------------------- |
| -32602     | Invalid params                            | Filter omits both mint and programId, or the owner Pubkey is malformed |
| -32603     | Internal error                            | Node failed to scan token accounts for the owner                       |
| -32016     | Minimum context slot has not been reached | The node has not yet processed minContextSlot                          |
| -32005     | Node is unhealthy                         | The node has fallen behind the cluster tip                             |
