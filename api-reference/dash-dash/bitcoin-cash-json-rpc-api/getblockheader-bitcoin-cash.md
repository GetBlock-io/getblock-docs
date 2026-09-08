---
description: >-
  Example code for the getblockheader JSON-RPC method. Complete guide on how to
  use getblockheader JSON-RPC in GetBlock Web3 documentation.
---

# getblockheader - Bitcoin Cash

This method returns the header of a block given its hash. With verbose set to false it returns the hex-encoded header; with true it returns a JSON object of header fields.

## Parameters

| Parameter | Type    | Required | Description                                                      |
| --------- | ------- | -------- | ---------------------------------------------------------------- |
| blockhash | string  | Yes      | The block hash                                                   |
| verbose   | boolean | No       | true for a JSON object, false for hex-encoded data. Default true |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getblockheader",
    "params": ["0000000000000000023a561e1ea370153aac5d1504726d1a039032831c05fcfc", true],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="bitcoinjs-lib" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const { data } = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
  jsonrpc: '2.0', method: 'getblockheader',
  params: ['0000000000000000023a561e1ea370153aac5d1504726d1a039032831c05fcfc', true], id: 'getblock.io'
});
console.log(data.result.height, data.result.time);
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
        'method': 'getblockheader',
        'params': ["0000000000000000023a561e1ea370153aac5d1504726d1a039032831c05fcfc", true],
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
            "method": "getblockheader",
            "params": ["0000000000000000023a561e1ea370153aac5d1504726d1a039032831c05fcfc", true],
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
    "error": null,
    "id": "getblock.io",
    "result": {
        "hash": "0000000000000000023a561e1ea370153aac5d1504726d1a039032831c05fcfc",
        "confirmations": 1197,
        "height": 684634,
        "version": 1073733632,
        "versionHex": "3fffe000",
        "merkleroot": "d14c9f467c4bdd5135837696150ab5f52f3f5043de324ca4e5766b195b9f8f37",
        "time": 1617180599,
        "mediantime": 1617176373,
        "nonce": 3669423616,
        "bits": "170cdf6f",
        "difficulty": 21865558044610.55,
        "chainwork": "00000000000000000000000000000000000000001b633a711a2334c78a29bb40",
        "nTx": 2815,
        "previousblockhash": "000000000000000001d3a318372df5d1eec54462a0d7471ae1cdf49838f793dd",
        "nextblockhash": "000000000000000000006d8e1eb870bd281b30ed621acf6b8d6af2a3c7ab61f1"
    }
}
```

## Response Parameters

| Parameter | Type         | Description                                                          |
| --------- | ------------ | -------------------------------------------------------------------- |
| error     | null\|object | Error object when the call fails, otherwise null                     |
| id        | string       | Request identifier matching the request                              |
| result    | object       | Block header object when verbose is true, or a hex string when false |

### Result Object

| Field             | Type    | Description                             |
| ----------------- | ------- | --------------------------------------- |
| hash              | string  | Block hash                              |
| height            | numeric | Block height                            |
| merkleroot        | string  | Merkle root of the block's transactions |
| time              | numeric | Block time as a Unix timestamp          |
| nonce             | numeric | Nonce used in the proof-of-work         |
| bits              | string  | Compact form of the target threshold    |
| previousblockhash | string  | Hash of the parent block                |

## Use Cases

* **Lightweight Sync**: Fetch headers without downloading full block bodies
* **Proof of Work**: Read bits and nonce to verify a block's difficulty target
* **Chain Walking**: Follow previousblockhash to traverse the header chain
* **Timestamp Checks**: Read mediantime for time-based consensus rules

## Error Handling

| Error Code | Message           | Description                                      |
| ---------- | ----------------- | ------------------------------------------------ |
| -8         | Invalid parameter | blockhash is not a valid 64-character hex string |
| -5         | Block not found   | No block with the given hash exists on this node |
| -32603     | Internal error    | Node failed to read the block from disk          |
