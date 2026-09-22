---
description: >-
  Example code for the akash/deployment/v1beta3/groups/info REST method.
  Complete guide on how to use akash/deployment/v1beta3/groups/info REST
  method in GetBlock Web3 documentation.
---

# /akash/deployment/v1beta3/groups/info - Akash

Returns a single deployment group (a set of resource requirements within a deployment) by its owner, dseq, and gseq.

## Endpoint

```http
GET /akash/deployment/v1beta3/groups/info
```

## Query Parameters

| Parameter | Type   | Description         |
| --------- | ------ | ------------------- |
| id.owner  | string | Owner address       |
| id.dseq   | string | Deployment sequence |
| id.gseq   | string | Group sequence      |

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}akash/deployment/v1beta3/groups/info?id.owner=akash1nl7dg3xj9j2y6q7z0v4w8c2b5d1f3g6h9k2m4p&id.dseq=12345678&id.gseq=1"
```
{% endcode %}

## Response

```json
{
    "group": {
        "group_id": {
            "owner": "akash1nl7dg3xj9j2y6q7z0v4w8c2b5d1f3g6h9k2m4p",
            "dseq": "12345678",
            "gseq": 1
        },
        "state": "open",
        "group_spec": {
            "name": "westcoast",
            "resources": []
        }
    }
}
```

## Response Fields

| Field | Type   | Description                        |
| ----- | ------ | ---------------------------------- |
| group | object | Group id, state, and resource spec |

## Use Cases

* **Resource Detail**: Inspect a deployment's resource group

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
