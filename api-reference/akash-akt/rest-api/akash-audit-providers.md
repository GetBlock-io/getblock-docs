---
description: >-
  Example code for the akash/audit/v1/audit/attributes/list REST method.
  Complete guide on how to use akash/audit/v1/audit/attributes/list REST
  method in GetBlock Web3 documentation.
---

# /akash/audit/v1/audit/attributes/list - Akash

Returns provider attributes that have been signed (audited) by auditors, letting tenants trust provider claims such as region or hardware.

## Endpoint

```
GET /akash/audit/v1/audit/attributes/list
```

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}akash/audit/v1/audit/attributes/list?pagination.limit=1"
```
{% endcode %}

## Response

```json
{
    "providers": [
        {
            "owner": "akash1pvskd94jtkph9vgqpf6ecg2as8pvck9w5a7wga",
            "auditor": "akash1365yvmc4s7awdyj3n2sav7xfx76adc6dnmlx63",
            "attributes": [
                {
                    "key": "capabilities/cpu",
                    "value": "amd"
                }
            ]
        }
    ],
    "pagination": {
        "next_key": "ARQM+50/cZzmA5ax9ZByqtR7pAMV4hSOqEZvFYe65pJRmqHWeMk3tdbjTQ==",
        "total": "0"
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
