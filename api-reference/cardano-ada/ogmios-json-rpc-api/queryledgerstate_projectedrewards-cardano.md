---
description: >-
  Example code for the queryLedgerState_projectedRewards JSON-RPC method.
  Complete guide on how to use queryLedgerState_projectedRewards JSON-RPC in
  GetBlock Web3 documentation.
---

# queryLedgerState\_projectedRewards - Cardano

This method projects the rewards a set of stake credentials or ada amounts would earn, using a non-myopic model across stake pools.

## Parameters

| Parameter | Type  | Required | Description                              |
| --------- | ----- | -------- | ---------------------------------------- |
| stake     | array | No       | Ada amounts to project rewards for       |
| keys      | array | No       | Stake credentials to project rewards for |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "queryLedgerState/projectedRewards",
    "params": {
        "stake": [
            {
                "ada": {
                    "lovelace": 100000000000
                }
            }
        ]
    },
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({"jsonrpc": "2.0", "method": "queryLedgerState/projectedRewards", "params": {"stake": [{"ada": {"lovelace": 100000000000}}]}, "id": "getblock.io"})
    }
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.post(
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={"jsonrpc": "2.0", "method": "queryLedgerState/projectedRewards", "params": {"stake": [{"ada": {"lovelace": 100000000000}}]}, "id": "getblock.io"}
)

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "jsonrpc": "2.0",
    "method": "queryLedgerState/projectedRewards",
    "result": {
        "stake1uxy2lpan99fcnhhyzr8w8qk4dqz4mp7g6b8h3r2v5c9d0e1f2g3h4": {
            "pool1abc...": {
                "ada": {
                    "lovelace": 512340
                }
            }
        }
    },
    "id": "getblock.io"
}
```

## Response Parameters

| Field  | Type   | Description                                            |
| ------ | ------ | ------------------------------------------------------ |
| result | object | Projected rewards per credential per pool, in lovelace |

## Use Cases

* **Reward Estimation**: Show expected rewards before delegating
* **Pool Comparison**: Compare projected rewards across pools
* **Delegation Advice**: Recommend a pool from projected returns
* **Planning**: Model rewards for a hypothetical stake amount

## Error Handling

| Error Code | Message           | Description                                          |
| ---------- | ----------------- | ---------------------------------------------------- |
| -32602     | Invalid params    | A required field is missing or has the wrong type    |
| -32000     | Query unavailable | The query is not available in the current ledger era |
| -32603     | Internal error    | The node failed to answer the query                  |
