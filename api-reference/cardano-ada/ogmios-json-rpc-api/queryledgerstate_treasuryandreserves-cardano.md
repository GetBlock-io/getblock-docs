---
description: >-
  Example code for the queryLedgerState_treasuryAndReserves JSON-RPC method.
  Complete guide on how to use queryLedgerState_treasuryAndReserves JSON-RPC in
  GetBlock Web3 documentation.
---

# queryLedgerState\_treasuryAndReserves - Cardano

This method returns the current balances of the treasury and reserves ada pots, which fund rewards and governance actions.

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
    "method": "queryLedgerState/treasuryAndReserves",
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
        body: JSON.stringify({"jsonrpc": "2.0", "method": "queryLedgerState/treasuryAndReserves", "id": "getblock.io"})
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
    json={"jsonrpc": "2.0", "method": "queryLedgerState/treasuryAndReserves", "id": "getblock.io"}
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
    "method": "queryLedgerState/treasuryAndReserves",
    "result": {
        "treasury": {
            "ada": {
                "lovelace": 1444502385502327
            }
        },
        "reserves": {
            "ada": {
                "lovelace": 6216188192911211
            }
        }
    },
    "id": "getblock.io"
}
```

## Response Parameters

| Field    | Type   | Description                          |
| -------- | ------ | ------------------------------------ |
| treasury | object | Current treasury balance in lovelace |
| reserves | object | Current reserves balance in lovelace |

## Use Cases

* **Monetary Analysis**: Track treasury and reserves over time
* **Governance Modeling**: Estimate funds available for governance actions
* **Dashboards**: Display protocol pot balances
* **Issuance Studies**: Model reward funding from reserves

## Error Handling

| Error Code | Message           | Description                                          |
| ---------- | ----------------- | ---------------------------------------------------- |
| -32602     | Invalid params    | A required field is missing or has the wrong type    |
| -32000     | Query unavailable | The query is not available in the current ledger era |
| -32603     | Internal error    | The node failed to answer the query                  |
