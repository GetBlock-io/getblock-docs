---
description: >-
  Example code for the getProgramAccounts JSON-RPC method. Complete guide on how
  to use the getProgramAccounts JSON-RPC method in the GetBlock Web3
  documentation.
---

# getProgramAccounts - Solana

This method returns all accounts owned by a program, with optional filters on data size and byte content. This is the primary method for indexing program state such as SPL Token accounts and Anchor PDAs.

## Parameters

| Parameter | Type   | Required | Description                                                        |
| --------- | ------ | -------- | ------------------------------------------------------------------ |
| programId | string | Yes      | Base58-encoded Pubkey of the owning program                        |
| config    | object | No       | Configuration object controlling filters, encoding, and commitment |

### Config Object

| Field          | Type    | Required | Description                                                                 |
| -------------- | ------- | -------- | --------------------------------------------------------------------------- |
| encoding       | string  | No       | Data encoding: base58, base64, base64+zstd, or jsonParsed                   |
| commitment     | string  | No       | Commitment level: processed, confirmed, or finalized. Defaults to finalized |
| dataSlice      | object  | No       | Byte range to return, with offset and length fields                         |
| filters        | array   | No       | Up to 4 filter objects, each a dataSize or memcmp filter                    |
| withContext    | boolean | No       | Wrap the result in an RpcResponse context object                            |
| minContextSlot | number  | No       | Minimum slot the request can be evaluated at                                |

### Memcmp Filter

| Field    | Type   | Required | Description                                       |
| -------- | ------ | -------- | ------------------------------------------------- |
| offset   | number | Yes      | Byte offset into the account data to compare from |
| bytes    | string | Yes      | Data to match, base58 or base64 encoded           |
| encoding | string | No       | Encoding of the bytes field: base58 or base64     |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getProgramAccounts",
    "params": ["TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA", {"encoding": "jsonParsed", "filters": [{"dataSize": 165}, {"memcmp": {"offset": 0, "bytes": "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v"}}]}],
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

const accounts = await connection.getParsedProgramAccounts(
  new PublicKey('TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA'),
  {
    filters: [
      { dataSize: 165 },
      { memcmp: { offset: 0, bytes: 'EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v' } }
    ]
  }
);

console.log(accounts.length);
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
        'method': 'getProgramAccounts',
        'params': ["TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA", {"encoding": "jsonParsed", "filters": [{"dataSize": 165}, {"memcmp": {"offset": 0, "bytes": "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v"}}]}],
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
            "method": "getProgramAccounts",
            "params": ["TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA", {"encoding": "jsonParsed", "filters": [{"dataSize": 165}, {"memcmp": {"offset": 0, "bytes": "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v"}}]}],
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
    "result": [
        {
            "account": {
                "data": [
                    "",
                    "base64"
                ],
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
```

## Response Parameters

| Parameter | Type   | Description                                                                        |
| --------- | ------ | ---------------------------------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                                                  |
| id        | string | Request identifier matching the request                                            |
| result    | array  | Array of account objects owned by the program, each with pubkey and account fields |

### Result Entry

| Field   | Type   | Description                                                                 |
| ------- | ------ | --------------------------------------------------------------------------- |
| pubkey  | string | Base58 Pubkey of the account                                                |
| account | object | Account object with lamports, owner, data, executable, rentEpoch, and space |

## Use Cases

* **Token Holder Indexes**: Enumerate every SPL Token account for a given mint
* **Anchor Discriminators**: Filter PDAs by the 8-byte account discriminator at offset 0
* **Order Book Crawls**: List open orders accounts owned by a DEX program
* **Stake Analysis**: Query stake accounts delegated to a specific vote account
* **Protocol Dashboards**: Rebuild protocol state without an off-chain indexer

## Error Handling

| Error Code | Message                                   | Description                                                          |
| ---------- | ----------------------------------------- | -------------------------------------------------------------------- |
| -32602     | Invalid params                            | More than 4 filters, malformed memcmp bytes, or unsupported encoding |
| -32603     | Internal error                            | Scan exceeded the node's account scan limits                         |
| -32016     | Minimum context slot has not been reached | The node has not yet processed minContextSlot                        |
| -32005     | Node is unhealthy                         | The node has fallen behind the cluster tip                           |
