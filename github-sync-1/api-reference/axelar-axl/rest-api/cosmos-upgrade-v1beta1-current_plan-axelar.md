---
description: >-
  Example code for the cosmos/upgrade/v1beta1/current_plan REST method. Complete
  guide on how to use cosmos/upgrade/v1beta1/current_plan REST method in
  GetBlock Web3 documentation.
---

# /cosmos/upgrade/v1beta1/current\_plan - Axelar

Returns the currently scheduled chain upgrade plan, if any.

## Endpoint

```http
GET /cosmos/upgrade/v1beta1/current_plan
```

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}cosmos/upgrade/v1beta1/current_plan"
```
{% endcode %}

## Response

```json
{
    "plan": {
        "name": "v0.38.0",
        "height": "20000000",
        "info": ""
    }
}
```

## Response Fields

| Field | Type   | Description                     |
| ----- | ------ | ------------------------------- |
| plan  | object | Scheduled upgrade plan, or null |

## Use Cases

* **Ops**: Track scheduled upgrades

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
