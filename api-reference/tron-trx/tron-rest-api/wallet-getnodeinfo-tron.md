---
description: >-
  Example code for the getnodeinfo REST method. Complete guide on how to use
  getnodeinfo REST method in GetBlock Web3 documentation.
---

# /wallet/getnodeinfo - Tron

This endpoint returns diagnostic information about the node, including its version, active connections, and block-sync state.

## Parameters

This endpoint does not require parameters.

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/wallet/getnodeinfo' \
--header 'Content-Type: application/json' \
--data-raw '{}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/wallet/getnodeinfo',
    {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({})
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/wallet/getnodeinfo',
    headers={'Content-Type': 'application/json'},
    json={}
)

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
  "beginSyncNum": 68000000,
  "block": "Num:68000123,ID:0000...",
  "solidityBlock": "Num:68000100,ID:0000...",
  "currentConnectCount": 30,
  "activeConnectCount": 15,
  "totalFlow": 123456789,
  "configNodeInfo": {
    "codeVersion": "4.7.4",
    "p2pVersion": "11111"
  }
}
```

## Response Parameters

| Field                      | Type    | Description                                  |
| -------------------------- | ------- | -------------------------------------------- |
| block                      | string  | The node's current head block                |
| solidityBlock              | string  | The node's latest confirmed (Solidity) block |
| currentConnectCount        | integer | Number of peer connections                   |
| configNodeInfo.codeVersion | string  | The java-tron software version               |

## Use Cases

* **Health Checks**: Confirm a node is connected and syncing
* **Confirmation Lag**: Compare head and Solidity block heights
* **Version Reporting**: Read the java-tron version for support
* **Peer Monitoring**: Track peer connection counts

## Error Handling

| HTTP Status | Message        | Description                                                                            |
| ----------- | -------------- | -------------------------------------------------------------------------------------- |
| 200         | OK             | The request succeeded; some TRON errors are returned in the 200 body as an Error field |
| 400         | Bad request    | The request body is malformed or a required field is missing                           |
| 500         | Internal error | The node failed to process the request                                                 |
