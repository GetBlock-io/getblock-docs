---
description: >-
  Example code for the queryLedgerState_eraSummaries JSON-RPC method. Complete
  guide on how to use queryLedgerState_eraSummaries JSON-RPC in GetBlock Web3
  documentation.
---

# queryLedgerState\_eraSummaries - Cardano

This method returns summaries of all known eras, giving the parameters needed to convert between slots, epochs, and wall-clock time across hard forks.

## Parameters

* None

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "queryLedgerState/eraSummaries",
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
        body: JSON.stringify({"jsonrpc": "2.0", "method": "queryLedgerState/eraSummaries", "id": "getblock.io"})
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
    json={"jsonrpc": "2.0", "method": "queryLedgerState/eraSummaries", "id": "getblock.io"}
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
    "method": "queryLedgerState/eraSummaries",
    "result": [
        {
            "start": {
                "time": {
                    "seconds": 0
                },
                "slot": 0,
                "epoch": 0
            },
            "end": {
                "time": {
                    "seconds": 89856000
                },
                "slot": 4492800,
                "epoch": 208
            },
            "parameters": {
                "epochLength": 21600,
                "slotLength": {
                    "milliseconds": 20000
                },
                "safeZone": 4320
            }
        }
    ],
    "id": "getblock.io"
}
```

## Response Parameters

| Field      | Type   | Description                                          |
| ---------- | ------ | ---------------------------------------------------- |
| start      | object | Bound at which the era begins                        |
| end        | object | Bound at which the era ends                          |
| parameters | object | Epoch length, slot length, and safe zone for the era |

## Use Cases

* **Time Conversion**: Convert any slot to time across all eras
* **Scheduling**: Compute future epoch and slot boundaries
* **Wallet Backends**: Translate slots to dates for transaction history
* **Analytics**: Normalize timestamps across hard forks

## Error Handling

| Error Code | Message           | Description                                          |
| ---------- | ----------------- | ---------------------------------------------------- |
| -32602     | Invalid params    | A required field is missing or has the wrong type    |
| -32000     | Query unavailable | The query is not available in the current ledger era |
| -32603     | Internal error    | The node failed to answer the query                  |
