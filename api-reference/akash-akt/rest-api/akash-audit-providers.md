---
description: >-
  Example code for the akash/audit/v1beta3/audit/attributes/list REST method.
  Complete guide on how to use akash/audit/v1beta3/audit/attributes/list REST
  method in GetBlock Web3 documentation.
---

# /akash/audit/v1beta3/audit/attributes/list - Akash

Returns provider attributes that have been signed (audited) by auditors, letting tenants trust provider claims such as region or hardware.

## Endpoint

```
GET /akash/audit/v1beta3/audit/attributes/list
```

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}akash/audit/v1beta3/audit/attributes/list"
```
{% endcode %}

## Response

```json
{
    "providers": [
        {
            "owner": "akashvaloper1nl7dg3xj9j2y6q7z0v4w8c2b5d1f3g6h9k2m4p",
            "auditor": "akash1nl7dg3xj9j2y6q7z0v4w8c2b5d1f3g6h9k2m4p",
            "attributes": [
                {
                    "key": "region",
                    "value": "us-west"
                }
            ]
        }
    ],
    "pagination": {
        "total": "1"
    }
}
```

## Response Fields

| Field     | Type  | Description                                 |
| --------- | ----- | ------------------------------------------- |
| providers | array | Audited attribute sets per provider/auditor |

## Use Cases

* **Trust**: Filter providers by audited attributes
* **Placement**: Require audited claims

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
