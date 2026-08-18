---
description: >-
  Example code for the queryLedgerState_rewardsProvenance JSON-RPC method.
  Complete guide on how to use queryLedgerState_rewardsProvenance JSON-RPC in
  GetBlock Web3 documentation.
---

# queryLedgerState\_rewardsProvenance - Cardano

This method returns the data used to compute rewards for the current epoch, giving insight into how pool rewards were derived.

## Parameters

This method does not require parameters.

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "queryLedgerState/rewardsProvenance",
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({"jsonrpc": "2.0", "method": "queryLedgerState/rewardsProvenance", "id": "getblock.io"})
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={"jsonrpc": "2.0", "method": "queryLedgerState/rewardsProvenance", "id": "getblock.io"}
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
    "method": "queryLedgerState/rewardsProvenance",
    "result": {
        "desiredNumberOfStakePools": 500,
        "totalRewards": {
            "ada": {
                "lovelace": 12345678900000
            }
        },
        "activeStake": {
            "ada": {
                "lovelace": 23000000000000000
            }
        },
        "efficiency": "97/100"
    },
    "id": "getblock.io"
}
```

## Response Parameters

| Field                     | Type    | Description                          |
| ------------------------- | ------- | ------------------------------------ |
| desiredNumberOfStakePools | integer | The target pool count parameter      |
| totalRewards              | object  | Total rewards available in the epoch |
| activeStake               | object  | Total active stake in the epoch      |
| efficiency                | string  | Overall reward efficiency as a ratio |

## Use Cases

* **Reward Analysis**: Audit how epoch rewards were computed
* **Pool Research**: Study the inputs behind pool reward shares
* **Dashboards**: Report total rewards and active stake per epoch
* **Modeling**: Calibrate reward models against on-chain provenance

## Error Handling

| Error Code | Message           | Description                                          |
| ---------- | ----------------- | ---------------------------------------------------- |
| -32602     | Invalid params    | A required field is missing or has the wrong type    |
| -32000     | Query unavailable | The query is not available in the current ledger era |
| -32603     | Internal error    | The node failed to answer the query                  |
