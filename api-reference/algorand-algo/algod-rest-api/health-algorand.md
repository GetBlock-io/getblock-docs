---
description: >-
  Example code for the /health REST method. Complete guide on how to use the
  /health REST method in the GetBlock Web3 documentation.
---

# /health - Algorand

Returns HTTP 200 with an empty body when the algod node is healthy and able to serve requests. Used as a lightweight liveness probe.

## Endpoint

```http
GET /health
```

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}health"
```
{% endcode %}

## Response

```json
{}
```

## Response Fields

| Field    | Type | Description                     |
| -------- | ---- | ------------------------------- |
| (status) | http | 200 OK when the node is healthy |

## Use Cases

* **Liveness Probes**: Health-check the node in a load balancer
* **Monitoring**: Alert when the endpoint stops responding
* **Failover**: Route away from an unhealthy node

## Error Handling

| Error                     | Message          | Description                                       |
| ------------------------- | ---------------- | ------------------------------------------------- |
| 503 / Unavailable         | Node unavailable | The node is not ready to serve requests           |
| 403 / RBAC: access denied | Access denied    | The GetBlock access token is missing or incorrect |
