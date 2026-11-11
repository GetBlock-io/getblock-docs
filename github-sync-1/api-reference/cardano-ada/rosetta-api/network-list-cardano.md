---
description: >-
  Example code for the network/list JSON-RPC method. Complete guide on how to
  use network/list JSON-RPC in GetBlock Web3 documentation.
---

# network/list - Cardano

This endpoint returns the network identifiers the Rosetta server supports. It is the first call a client makes to discover which networks are available.

## Parameters

The request body is a JSON object with the following fields.

| Field    | Type   | Required | Description                             |
| -------- | ------ | -------- | --------------------------------------- |
| metadata | object | No       | Optional metadata object, usually empty |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/network/list' \
--header 'Content-Type: application/json' \
--data-raw '{
    "metadata": {}
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/network/list',
    {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({"metadata": {}})
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/network/list',
    headers={'Content-Type': 'application/json'},
    json={"metadata": {}}
)

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "network_identifiers": [
        {
            "blockchain": "cardano",
            "network": "mainnet"
        }
    ]
}
```

## Response Parameters

| Field                | Type  | Description                                                 |
| -------------------- | ----- | ----------------------------------------------------------- |
| network\_identifiers | array | Supported networks, each with a blockchain and network name |

## Use Cases

* **Discovery**: Discover which networks the endpoint serves
* **Configuration**: Confirm the mainnet identifier before other calls
* **Multi-Network Clients**: Enumerate networks a client can target
* **Health Checks**: Confirm the Rosetta server is responding

## Error Handling

| Error Code | Message         | Description                                              |
| ---------- | --------------- | -------------------------------------------------------- |
| 1          | Invalid request | The request body is malformed or missing required fields |
| 4          | Block not found | The requested block does not exist on this node          |
| 5          | Internal error  | The node failed to answer the request                    |
