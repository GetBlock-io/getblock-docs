---
description: >-
  Example code for the block_search JSON-RPC method. Complete guide on how to
  use block_search JSON-RPC in GetBlock Web3 documentation.
---

# block\_search - Akash

Returns blocks matching a block-event query (for example by begin/end-block events), paginated. Requires block indexing.

## Parameters

| Parameter | Type   | Required | Description       |
| --------- | ------ | -------- | ----------------- |
| query     | string | Yes      | Block-event query |
| page      | string | Optional | Page              |
| per\_page | string | Optional | Results per page  |
| order\_by | string | Optional | asc or desc       |

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
    "method": "block_search",
    "params": {"query": "block.height > 28742277", "page": "1", "per_page": "2", "order_by": "desc"}
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'block_search', params: {"query": "block.height > 28742277", "page": "1", "per_page": "2", "order_by": "desc"} }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'block_search', 'params': {"query": "block.height > 19000000", "page": "1", "per_page": "20", "order_by": "desc"}})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"block_search","params":{"query": "block.height > 28742277", "page": "1", "per_page": "2", "order_by": "desc"}})).send().await?.json::<Value>().await?;
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
        "blocks": [
            {
                "block_id": {
                    "hash": "0C5667C867E028A1049B9C76934EC79DB70FA3971ED4DF2F8E036930F8079747",
                    "parts": {
                        "total": 1,
                        "hash": "31DB4D11C2414533FCC72937D3C04BB50B1AC82C7207894F7D8CF80010B3A61D"
                    }
                },
                "block": {
                    "header": {
                        "version": {
                            "block": "11"
                        },
                        "chain_id": "akashnet-2",
                        "height": "28742443",
                        "time": "2026-09-22T18:23:46.329052101Z",
                        "last_block_id": {
                            "hash": "A81B759AC891E761A0C534C741A1F0AADB2E5D8D7CB14729AFEF08685A2504F7",
                            "parts": {
                                "total": 1,
                                "hash": "97A604BB432B7CE8EE3EADEC4938DC512922CC8156DFDF7DDC38302ED4F22E90"
                            }
                        },
                        "last_commit_hash": "41D1A216FFBF869D53FFF9244EC204B62504505893119BAD35FDD160B3F8A056",
                        "data_hash": "996406B236389E6A984FA97CD5D9F5422B6B819A67540B347FEFA03B2DA7A39E",
                        "validators_hash": "44EF9BB4CC678BA80E9FE6AD3789DDDBEFC12CCE7F281A6D93A4ACD802CC6252",
                        "next_validators_hash": "44EF9BB4CC678BA80E9FE6AD3789DDDBEFC12CCE7F281A6D93A4ACD802CC6252",
                        "consensus_hash": "048091BC7DDC283F77BFBF91D73C44DA58C3DF8A9CBC867405D8B7F3DAADA22F",
                        "app_hash": "1E9187CAEC9826A2FA2AB835BF545AA62323345072E4BBAFCAFFE1A162201CDF",
                        "last_results_hash": "6F945892B7D476B194257C29E7DEF1E1129BA5DFBB6103F5AA4DEA2CDAC36B8B",
                        "evidence_hash": "E3B0C44298FC1C149AFBF4C8996FB92427AE41E4649B934CA495991B7852B855",
                        "proposer_address": "408B9882EAA1E636BD48998340BABE0153050CC3"
                    },
                    "data": {
                        "txs": [
                            "CroICpMICiQvY29zbXdhc20ud2FzbS52MS5Nc2dFeGVjdXRlQ29udHJhY3QS6gcKLGFrYXNoMXFhZnZl..."
                        ]
                    },
                    "evidence": {
                        "evidence": []
                    },
                    "last_commit": {
                        "height": "28742442",
                        "round": 0,
                        "block_id": {
                            "hash": "A81B759AC891E761A0C534C741A1F0AADB2E5D8D7CB14729AFEF08685A2504F7",
                            "parts": {
                                "total": 1,
                                "hash": "97A604BB432B7CE8EE3EADEC4938DC512922CC8156DFDF7DDC38302ED4F22E90"
                            }
                        },
                        "signatures": [
                            {
                                "block_id_flag": 2,
                                "validator_address": "B1852D17FA66B5382A8F770725CC5B228B357750",
                                "timestamp": "2026-09-22T18:23:46.329052101Z",
                                "signature": "2P8udD1+d5Z/3W62DCDIq8RHzcQfvrwtq1OJQm8zzgcfNSXEDglYPSxjOdaIGgtIfzFxVIn8YL94mJDAJflPBw=="
                            }
                        ]
                    }
                }
            }
        ],
        "total_count": "166"
    }
}
```

## Response Fields

| Field        | Type   | Description     |
| ------------ | ------ | --------------- |
| blocks       | array  | Matching blocks |
| total\_count | string | Total matches   |

## Use Cases

* **Event Search**: Find blocks by begin/end-block events
* **Indexing**: Locate blocks by criteria

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| -32603 / Internal error   | Internal error | Block indexing disabled or bad query              |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
