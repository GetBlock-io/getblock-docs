---
description: >-
  Example code for the healthy JSON-RPC method. Complete guide on how to use
  healthy JSON-RPC in GetBlock Web3 documentation.
---

# healthy - Movement

Returns a health check for the node. Optionally takes a `duration_secs` query parameter to assert the node has committed a transaction within that window.

## Endpoint

```http
GET /-/healthy
```

## Query Parameters

| Parameter      | Type   | Description                                                                    |
| -------------- | ------ | ------------------------------------------------------------------------------ |
| duration\_secs | string | Max staleness in seconds the node is allowed before being considered unhealthy |

## Example

{% code overflow="wrap" %}
```bash
export MOVEMENT_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${MOVEMENT_REST}-/healthy?duration_secs=10"
```
{% endcode %}

## Response

```json
{
    "message": "success"
}
```

## Response Fields

| Field   | Type   | Description                        |
| ------- | ------ | ---------------------------------- |
| message | string | "success" when the node is healthy |

## Use Cases

* **Monitoring**: Liveness probe for load balancers
* **Staleness Check**: Assert recent commit activity

## Error Handling

| Error                     | Message       | Description                                                 |
| ------------------------- | ------------- | ----------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                |
| 404 / Not found           | Not found     | The account, block, transaction, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect           |
