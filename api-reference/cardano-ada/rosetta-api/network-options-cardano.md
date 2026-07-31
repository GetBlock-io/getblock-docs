---
description: >-
  Example code for the network/options JSON-RPC method. Complete guide on how to
  use network/options JSON-RPC in GetBlock Web3 documentation.
---

# network/options - Cardano

This endpoint returns the version information and the allowed network-specific types, such as operation types, statuses, and errors, for a network.

## Parameters

The request body is a JSON object with the following fields.

| Field               | Type   | Required | Description              |
| ------------------- | ------ | -------- | ------------------------ |
| network\_identifier | object | Yes      | The network to query     |
| metadata            | object | No       | Optional metadata object |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/network/options' \
--header 'Content-Type: application/json' \
--data-raw '{
    "network_identifier": {
        "blockchain": "cardano",
        "network": "mainnet"
    },
    "metadata": {}
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://go.getblock.io/<ACCESS-TOKEN>/network/options',
    {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({"network_identifier": {"blockchain": "cardano", "network": "mainnet"}, "metadata": {}})
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
    'https://go.getblock.io/<ACCESS-TOKEN>/network/options',
    headers={'Content-Type': 'application/json'},
    json={"network_identifier": {"blockchain": "cardano", "network": "mainnet"}, "metadata": {}}
)

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "version": {
        "rosetta_version": "1.4.10",
        "node_version": "8.9.0",
        "middleware_version": "1.2.0"
    },
    "allow": {
        "operation_statuses": [
            {
                "status": "success",
                "successful": true
            }
        ],
        "operation_types": [
            "input",
            "output",
            "stakeKeyRegistration",
            "stakeDelegation",
            "withdrawal"
        ],
        "errors": [
            {
                "code": 1,
                "message": "Invalid request",
                "retriable": false
            }
        ],
        "historical_balance_lookup": true
    }
}
```

## Response Parameters

| Field                             | Type    | Description                               |
| --------------------------------- | ------- | ----------------------------------------- |
| version                           | object  | Rosetta, node, and middleware versions    |
| allow.operation\_types            | array   | Operation types the network uses          |
| allow.operation\_statuses         | array   | Possible operation statuses               |
| allow.errors                      | array   | Errors the server can return              |
| allow.historical\_balance\_lookup | boolean | Whether historical balances are supported |

## Use Cases

* **Capability Discovery**: Learn which operation types the network supports
* **Version Checks**: Confirm the node and Rosetta versions
* **Error Handling**: Read the full error catalog for the network
* **Historical Support**: Detect whether historical balance lookups are allowed

## Error Handling

| Error Code | Message         | Description                                              |
| ---------- | --------------- | -------------------------------------------------------- |
| 1          | Invalid request | The request body is malformed or missing required fields |
| 4          | Block not found | The requested block does not exist on this node          |
| 5          | Internal error  | The node failed to answer the request                    |
