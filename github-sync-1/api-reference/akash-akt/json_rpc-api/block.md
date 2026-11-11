---
description: >-
  Example code for the block JSON-RPC method. Complete guide on how to use
  block JSON-RPC in GetBlock Web3 documentation.
---

# block - Akash

Returns the block at a height, or the latest block when height is omitted, including its header, transactions, and commit.

{% hint style="warning" %}
**Shared Akash nodes are pruned.** Heights below roughly 26,980,292 are not retained and return
`-32603 Internal error` with a message naming the lowest available height. Read that floor from
[status](status.md) under `sync_info.earliest_block_height` before requesting historical data.
{% endhint %}

## Parameters

| Parameter | Type   | Required | Description                   |
| --------- | ------ | -------- | ----------------------------- |
| height    | string | Optional | Block height; omit for latest |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "block",
    "params": {"height": "28742282"}
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'block', params: {"height": "28742282"} }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'block', 'params': {"height": "19500000"}})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"block","params":{"height": "28742282"}})).send().await?.json::<Value>().await?;
    println!("{}", res["result"]);
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
        "block_id": {
            "hash": "8F57F092FDEEFA889F07F20182E74627F64CAD8EE2AA43D39CFE36C40FC46FB8",
            "parts": {
                "total": 1,
                "hash": "899C82D6CD41758912C6BD7CCDFAC31DBD08A2874B1167F9157E3251DA0A9C4B"
            }
        },
        "block": {
            "header": {
                "version": {
                    "block": "11"
                },
                "chain_id": "akashnet-2",
                "height": "28742282",
                "time": "2026-09-22T18:08:00.55315049Z",
                "last_block_id": {
                    "hash": "19D4712100648F95B35AC2F1CFB97F8134933549F99427AA5742D234376C1456",
                    "parts": {
                        "total": 1,
                        "hash": "092A941C2935B95ADC3B43FB447CD915AF7C723795B553798F5AEBD1494896F7"
                    }
                },
                "last_commit_hash": "60A5AC33FFA551E05B8C4FF1B7EF877A954067A6C9968C9CE3B5764D73C0DB5D",
                "data_hash": "10CD0CCD9E24F3C3AE818DC447F12FCB021F7C0E22294DB400A41BE532383EF5",
                "validators_hash": "98F9BA76ED248B252E5F94DFCC8999D279946F6E668B36876BFAF576BC0692BF",
                "next_validators_hash": "98F9BA76ED248B252E5F94DFCC8999D279946F6E668B36876BFAF576BC0692BF",
                "consensus_hash": "048091BC7DDC283F77BFBF91D73C44DA58C3DF8A9CBC867405D8B7F3DAADA22F",
                "app_hash": "C6BD49838677A50AA4A5CDD3C9601126B0E5B411F469E20E9E7DBBEAC6AF5327",
                "last_results_hash": "58C021D229E6FA80FC74D6590CAA50BAEDB7BB1C7CD2DDC4727E4507C9FA5A52",
                "evidence_hash": "E3B0C44298FC1C149AFBF4C8996FB92427AE41E4649B934CA495991B7852B855",
                "proposer_address": "25B40FD5AE2AC26B4382B746F6CDDB8C73CE6025"
            },
            "data": {
                "txs": [
                    "CrkICpMICiQvY29zbXdhc20ud2FzbS52MS5Nc2dFeGVjdXRlQ29udHJhY3QS6gcKLGFrYXNoMXFhZnZl..."
                ]
            },
            "evidence": {
                "evidence": []
            },
            "last_commit": {
                "height": "28742281",
                "round": 0,
                "block_id": {
                    "hash": "19D4712100648F95B35AC2F1CFB97F8134933549F99427AA5742D234376C1456",
                    "parts": {
                        "total": 1,
                        "hash": "092A941C2935B95ADC3B43FB447CD915AF7C723795B553798F5AEBD1494896F7"
                    }
                },
                "signatures": [
                    {
                        "block_id_flag": 2,
                        "validator_address": "B1852D17FA66B5382A8F770725CC5B228B357750",
                        "timestamp": "2026-09-22T18:08:00.481877746Z",
                        "signature": "pF5R7C9ldKCO4mguxDO6tr9GWha0ku4GsLfLWBOZX9wlkFZWBftstO1nYDInHO8/1HLWnqJz5MYfbIixDtGUAg=="
                    }
                ]
            }
        }
    }
}
```

## Response Fields

| Field          | Type   | Description                      |
| -------------- | ------ | -------------------------------- |
| block.header   | object | Chain id, height, time, proposer |
| block.data.txs | array  | Base64-encoded transactions      |

## Use Cases

* **Block Inspection**: Read a block's txs and header
* **Following**: Poll the latest block
* **Tx Extraction**: Decode base64 txs

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| -32603 / Internal error   | Internal error | Height out of range                               |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
