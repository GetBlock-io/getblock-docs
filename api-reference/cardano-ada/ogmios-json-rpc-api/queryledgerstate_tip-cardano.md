---
description: >-
  Example code for the queryLedgerState_tip JSON-RPC method. Complete guide on
  how to use queryLedgerState_tip JSON-RPC in GetBlock Web3 documentation.
---

# queryLedgerState\_tip - Cardano

This method returns the ledger tip: the slot and block hash of the most recent block reflected in the ledger state.

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
    "method": "queryLedgerState/tip",
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
        body: JSON.stringify({"jsonrpc": "2.0", "method": "queryLedgerState/tip", "id": "getblock.io"})
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
    json={"jsonrpc": "2.0", "method": "queryLedgerState/tip", "id": "getblock.io"}
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
    "method": "queryLedgerState/tip",
    "result": {
        "slot": 123456789,
        "id": "3e6f2d8c9a1b4e7f0c2d5a8b1e4f7a0c3d6b9e2f5a8c1d4e7b0f3a6c9d2e5b8f"
    },
    "id": "getblock.io"
}
```

## Response Parameters

| Field | Type    | Description                            |
| ----- | ------- | -------------------------------------- |
| slot  | integer | Absolute slot number of the ledger tip |
| id    | string  | Block hash of the ledger tip           |

## Use Cases

* **State Anchoring**: Record the point a ledger query was answered against
* **Consistency Checks**: Confirm several queries share the same ledger tip
* **Sync Verification**: Compare ledger tip against the network tip
* **Snapshotting**: Pin analysis to a specific ledger point

## Error Handling

| Error Code | Message           | Description                                          |
| ---------- | ----------------- | ---------------------------------------------------- |
| -32602     | Invalid params    | A required field is missing or has the wrong type    |
| -32000     | Query unavailable | The query is not available in the current ledger era |
| -32603     | Internal error    | The node failed to answer the query                  |
