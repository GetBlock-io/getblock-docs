---
description: >-
  Example code for the queryLedgerState_epoch JSON-RPC method. Complete guide on
  how to use queryLedgerState_epoch JSON-RPC in GetBlock Web3 documentation.
---

# queryLedgerState\_epoch - Cardno

This method returns the current epoch number of the ledger. Epochs group slots into the units used for reward and parameter cycles.

## Parameters

* none

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "queryLedgerState/epoch",
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
        body: JSON.stringify({"jsonrpc": "2.0", "method": "queryLedgerState/epoch", "id": "getblock.io"})
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
    json={"jsonrpc": "2.0", "method": "queryLedgerState/epoch", "id": "getblock.io"}
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
    "method": "queryLedgerState/epoch",
    "result": 507,
    "id": "getblock.io"
}
```

## Response Parameters

| Field  | Type    | Description          |
| ------ | ------- | -------------------- |
| result | integer | Current epoch number |

## Use Cases

* **Reward Cycles**: Align reward calculations to epoch boundaries
* **Parameter Windows**: Detect when protocol parameters may change
* **Scheduling**: Trigger epoch-based jobs
* **Display**: Show the current epoch on a dashboard

## Error Handling

| Error Code | Message           | Description                                          |
| ---------- | ----------------- | ---------------------------------------------------- |
| -32602     | Invalid params    | A required field is missing or has the wrong type    |
| -32000     | Query unavailable | The query is not available in the current ledger era |
| -32603     | Internal error    | The node failed to answer the query                  |
