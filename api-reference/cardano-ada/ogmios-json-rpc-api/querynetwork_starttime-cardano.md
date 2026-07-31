---
description: >-
  Example code for the queryNetwork_startTime JSON-RPC method. Complete guide on
  how to use queryNetwork_startTime JSON-RPC in GetBlock Web3 documentation.
---

# queryNetwork\_startTime - Cardano

This method returns the UTC start time of the network, the moment the first block could have been produced. It is a fixed genesis constant.

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
    "method": "queryNetwork/startTime",
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
        body: JSON.stringify({"jsonrpc": "2.0", "method": "queryNetwork/startTime", "id": "getblock.io"})
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
    json={"jsonrpc": "2.0", "method": "queryNetwork/startTime", "id": "getblock.io"}
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
    "method": "queryNetwork/startTime",
    "result": "2017-09-23T21:44:51Z",
    "id": "getblock.io"
}
```

## Response Parameters

| Field  | Type   | Description                                     |
| ------ | ------ | ----------------------------------------------- |
| result | string | Network start time as an ISO 8601 UTC timestamp |

## Use Cases

* **Slot Conversion**: Convert absolute slots to wall-clock time
* **Era Math**: Anchor era-length calculations to genesis
* **Display**: Show the network's age on a status page
* **Validation**: Confirm the node is on the expected network by its start time

## Error Handling

| Error Code | Message           | Description                                          |
| ---------- | ----------------- | ---------------------------------------------------- |
| -32602     | Invalid params    | A required field is missing or has the wrong type    |
| -32000     | Query unavailable | The query is not available in the current ledger era |
| -32603     | Internal error    | The node failed to answer the query                  |
