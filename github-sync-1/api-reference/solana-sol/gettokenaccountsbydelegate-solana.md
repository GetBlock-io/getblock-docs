---
description: >-
  Example code for the getTokenAccountsByDelegate JSON-RPC method. Complete
  guide on how to use the getTokenAccountsByDelegate JSON-RPC method in the
  GetBlock Web3 documentation.
---

# getTokenAccountsByDelegate - Solana

This method returns all SPL Token accounts whose approved delegate matches the supplied address. Results can be narrowed to a single mint or a single token program.

## Parameters

| Parameter | Type   | Required | Description                                              |
| --------- | ------ | -------- | -------------------------------------------------------- |
| delegate  | string | Yes      | Base58-encoded Pubkey of the account delegate            |
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
    "method": "getTokenAccountsByDelegate",
    "params": ["EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v", {"programId": "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA"}, {"encoding": "jsonParsed"}],
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

const { value } = await connection.getParsedTokenAccountsByDelegate(
  new PublicKey('EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v'),
  { programId: new PublicKey('TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA') }
);

console.log(value);
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
        'method': 'getTokenAccountsByDelegate',
        'params': ["EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v", {"programId": "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA"}, {"encoding": "jsonParsed"}],
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
    const client = Client::new();

    let response = client
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "getTokenAccountsByDelegate",
            "params": ["EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v", {"programId": "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA"}, {"encoding": "jsonParsed"}],
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
        "value": []
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                                            |
| --------- | ------ | ---------------------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                                      |
| id        | string | Request identifier matching the request                                |
| result    | object | RPC response object where value is an array of matching token accounts |

### Value Entry

| Field   | Type   | Description                                                                 |
| ------- | ------ | --------------------------------------------------------------------------- |
| pubkey  | string | Base58 Pubkey of the token account                                          |
| account | object | Account object with lamports, owner, data, executable, rentEpoch, and space |

## Use Cases

* **Delegation Audits**: List every token account a protocol has been approved to move
* **Escrow Tracking**: Verify delegate approvals before executing a transfer
* **Revocation Flows**: Surface active delegations so a user can revoke them
* **Security Review**: Detect unexpected delegate authority on user token accounts

## Error Handling

| Error Code | Message           | Description                                            |
| ---------- | ----------------- | ------------------------------------------------------ |
| -32602     | Invalid params    | Filter omits both mint and programId, or supplies both |
| -32603     | Internal error    | Node failed to scan token accounts for the delegate    |
| -32005     | Node is unhealthy | The node has fallen behind the cluster tip             |
