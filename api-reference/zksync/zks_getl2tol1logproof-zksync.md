# zks\_getl2tol1logproof zksync

Retrieves the log proof for an L2 to L1 transaction. The log proof can only be generated after the corresponding L2 transaction has been finalized (executed) on L1. Use this proof to claim withdrawals on L1.

## Parameters

| Parameter | Type    | Required | Description                                  |
| --------- | ------- | -------- | -------------------------------------------- |
| txHash    | string  | Yes      | Transaction hash (32 bytes)                  |
| logIndex  | integer | No       | Index of the log (optional for EraVM chains) |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "zks_getL2ToL1LogProof",
    "params": [
        "0x2a1c6c74b184965c0cb015aae9ea134fd96215d2e4f4979cfec12563295f610e"
    ],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "zks_getL2ToL1LogProof",
    "params": [
        "0x2a1c6c74b184965c0cb015aae9ea134fd96215d2e4f4979cfec12563295f610e"
    ],
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers: {
        'Content-Type': 'application/json'
    },
    data: data
};

axios(config)
    .then(response => console.log(JSON.stringify(response.data, null, 2)))
    .catch(error => console.log(error));
```
{% endtab %}

{% tab title="Python (Requests)" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "zks_getL2ToL1LogProof",
    "params": [
        "0x2a1c6c74b184965c0cb015aae9ea134fd96215d2e4f4979cfec12563295f610e"
    ],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endtab %}

{% tab title="Rust (Reqwest)" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let payload = json!({
        "jsonrpc": "2.0",
        "method": "zks_getL2ToL1LogProof",
        "params": [
                "0x2a1c6c74b184965c0cb015aae9ea134fd96215d2e4f4979cfec12563295f610e"
        ],
        "id": "getblock.io"
});

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&payload)
        .send()
        .await?;

    let result: serde_json::Value = response.json().await?;
    println!("{:#?}", result);

    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "proof": [
            "0x8c48910df2ca7de509daf50b3182fcdf2dd6c422c6704054fd857d6c9516d6fc",
            "0xc5028885760b8b596c4fa11497c783752cb3a3fb3b8e6b52d7e54b9f1c63521e"
        ],
        "id": 0,
        "root": "0x920c63cb0066a08da45f0a9bf934517141bd72d8e5a51421a94b517bf49a0d39",
        "batchNumber": 21710
    }
}
```
{% endcode %}

## Response Parameters

| Field              | Type            | Description                                            |
| ------------------ | --------------- | ------------------------------------------------------ |
| result.proof       | array of string | Array of 32-byte hashes — the Merkle proof for the log |
| result.id          | integer         | Identifier of the log within the transaction           |
| result.root        | string          | Root hash anchoring the proof                          |
| result.batchNumber | integer         | L1 batch number where the log was executed             |

## Use Cases

* L2 → L1 withdrawals — required to finalize on L1
* Cross-chain messaging proofs
* Trust-minimized L2 → L1 message verification

## Error Handling

| Status Code | Error Message     | Cause                                                |
| ----------- | ----------------- | ---------------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                  |
| -32602      | Invalid params    | Request parameters are missing or malformed          |
| -32601      | Method not found  | Method does not exist or is not enabled on this node |
| 429         | Too Many Requests | Rate limit exceeded for your plan                    |

## SDK Integration

{% tabs %}
{% tab title="zksync-ethers (JavaScript)" %}
```javascript
import { Provider } from 'zksync-ethers';

const provider = new Provider('https://go.getblock.io/<ACCESS-TOKEN>/');

// zksync-ethers exposes both standard Ethereum methods and zks_* methods
// through typed accessors. For raw access:
const result = await provider.send('zks_getL2ToL1LogProof', ["0x2a1c6c74b184965c0cb015aae9ea134fd96215d2e4f4979cfec12563295f610e"]);
console.log(result);
```
{% endtab %}

{% tab title="zksync2-python (Python)" %}
```python
from zksync2.module.module_builder import ZkSyncBuilder

zk_web3 = ZkSyncBuilder.build('https://go.getblock.io/<ACCESS-TOKEN>/')

# zksync2-python exposes the JSON-RPC layer directly.
result = zk_web3.zksync._zks_endpoints if 'zks_getL2ToL1LogProof'.startswith('zks_') else None
# Or call any method generically:
result = zk_web3.provider.make_request('zks_getL2ToL1LogProof', ["0x2a1c6c74b184965c0cb015aae9ea134fd96215d2e4f4979cfec12563295f610e"])
print(result)
```
{% endtab %}
{% endtabs %}
