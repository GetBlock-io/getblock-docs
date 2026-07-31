---
description: >-
  Example code for the queryNetwork_blockHeight JSON-RPC method. Complete guide
  on how to use queryNetwork_blockHeight JSON-RPC in GetBlock Web3
  documentation.
---

# queryNetwork\_blockHeight - Cardano

This method returns the height of the most recent block on the network the node is connected to. The height is the network's absolute chain length.

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
    "method": "queryNetwork/blockHeight",
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
        body: JSON.stringify({"jsonrpc": "2.0", "method": "queryNetwork/blockHeight", "id": "getblock.io"})
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
    json={"jsonrpc": "2.0", "method": "queryNetwork/blockHeight", "id": "getblock.io"}
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
    "method": "queryNetwork/blockHeight",
    "result": 13748117,
    "id": "getblock.io"
}
```

## Response Parameters

| Field  | Type    | Description                                                     |
| ------ | ------- | --------------------------------------------------------------- |
| result | integer | Current network block height, or the string "origin" at genesis |

## Use Cases

* **Sync Checks**: Compare against a local index to gauge sync progress
* **Confirmation Counts**: Subtract a transaction's block height from the tip
* **Status Pages**: Display the current chain height
* **Scheduling**: Trigger jobs on block-height intervals

## Error Handling

| Error Code | Message           | Description                                          |
| ---------- | ----------------- | ---------------------------------------------------- |
| -32602     | Invalid params    | A required field is missing or has the wrong type    |
| -32000     | Query unavailable | The query is not available in the current ledger era |
| -32603     | Internal error    | The node failed to answer the query                  |
