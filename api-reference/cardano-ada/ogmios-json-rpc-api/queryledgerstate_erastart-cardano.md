---
description: >-
  Example code for the queryLedgerState_eraStart JSON-RPC method. Complete guide
  on how to use queryLedgerState_eraStart JSON-RPC in GetBlock Web3
  documentation.
---

# queryLedgerState\_eraStart - Cardano

This method returns the information about the start of the current era: the epoch, slot, and absolute time at which it began.

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
    "method": "queryLedgerState/eraStart",
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
        body: JSON.stringify({"jsonrpc": "2.0", "method": "queryLedgerState/eraStart", "id": "getblock.io"})
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
    json={"jsonrpc": "2.0", "method": "queryLedgerState/eraStart", "id": "getblock.io"}
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
    "method": "queryLedgerState/eraStart",
    "result": {
        "time": {
            "seconds": 55814400
        },
        "slot": 55814400,
        "epoch": 208
    },
    "id": "getblock.io"
}
```

## Response Parameters

| Field | Type    | Description                                   |
| ----- | ------- | --------------------------------------------- |
| time  | object  | Relative time since network start, in seconds |
| slot  | integer | Absolute slot at which the era started        |
| epoch | integer | Epoch at which the era started                |

## Use Cases

* **Era Math**: Compute offsets within the current era
* **Slot Conversion**: Convert slots to time using era boundaries
* **Hard Fork Tracking**: Detect the boundary of the current era
* **Analytics**: Bucket activity by era

## Error Handling

| Error Code | Message           | Description                                          |
| ---------- | ----------------- | ---------------------------------------------------- |
| -32602     | Invalid params    | A required field is missing or has the wrong type    |
| -32000     | Query unavailable | The query is not available in the current ledger era |
| -32603     | Internal error    | The node failed to answer the query                  |
