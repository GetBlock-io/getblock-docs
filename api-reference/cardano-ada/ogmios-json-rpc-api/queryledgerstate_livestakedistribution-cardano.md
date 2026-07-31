---
description: >-
  Example code for the queryLedgerState_liveStakeDistribution JSON-RPC method.
  Complete guide on how to use queryLedgerState_liveStakeDistribution JSON-RPC
  in GetBlock Web3 documentation.
---

# queryLedgerState\_liveStakeDistribution - Cardano

This method returns the live stake distribution across stake pools, giving each pool's share of the total active stake.

## Parameters

This method does not require parameters.

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "queryLedgerState/liveStakeDistribution",
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
        body: JSON.stringify({"jsonrpc": "2.0", "method": "queryLedgerState/liveStakeDistribution", "id": "getblock.io"})
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
    json={"jsonrpc": "2.0", "method": "queryLedgerState/liveStakeDistribution", "id": "getblock.io"}
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
    "method": "queryLedgerState/liveStakeDistribution",
    "result": {
        "pool1abc...": {
            "stake": "12/1000000",
            "vrf": "b3c2...vrf-hash"
        }
    },
    "id": "getblock.io"
}
```

## Response Parameters

| Field | Type   | Description                                      |
| ----- | ------ | ------------------------------------------------ |
| stake | string | Pool's share of the total live stake, as a ratio |
| vrf   | string | The pool's VRF verification key hash             |

## Use Cases

* **Reward Modeling**: Estimate pool rewards from stake share
* **Decentralization Metrics**: Measure stake concentration across pools
* **Delegation Advice**: Weigh saturation when choosing a pool
* **Analytics**: Track stake movement between epochs

## Error Handling

| Error Code | Message           | Description                                          |
| ---------- | ----------------- | ---------------------------------------------------- |
| -32602     | Invalid params    | A required field is missing or has the wrong type    |
| -32000     | Query unavailable | The query is not available in the current ledger era |
| -32603     | Internal error    | The node failed to answer the query                  |
