---
description: >-
  Example code for the akash/provider/v1beta3/providers/{owner} REST method.
  Complete guide on how to use akash/provider/v1beta3/providers/{owner} REST
  method in GetBlock Web3 documentation.
---

# /akash/provider/v1beta3/providers/{owner} - Akash

Returns a single provider by its owner address, with its host URI, attributes, and contact info.

## Endpoint

```http
GET /akash/provider/v1beta3/providers/{owner}
```

## Path Parameters

| Parameter | Type   | Description                      |
| --------- | ------ | -------------------------------- |
| owner     | string | Provider owner address (akash1…) |

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}akash/provider/v1beta3/providers/akashvaloper1nl7dg3xj9j2y6q7z0v4w8c2b5d1f3g6h9k2m4p"
```
{% endcode %}

## Response

```json
{
    "provider": {
        "owner": "akashvaloper1nl7dg3xj9j2y6q7z0v4w8c2b5d1f3g6h9k2m4p",
        "host_uri": "https://provider.example.com:8443",
        "attributes": [
            {
                "key": "region",
                "value": "us-west"
            }
        ],
        "info": {
            "email": "ops@example.com",
            "website": "https://example.com"
        }
    }
}
```

## Response Fields

| Field               | Type   | Description             |
| ------------------- | ------ | ----------------------- |
| provider.host\_uri  | string | Provider gateway URI    |
| provider.attributes | array  | Capabilities and region |
| provider.info       | object | Contact info            |

## Use Cases

* **Provider Detail**: Render a provider page
* **Placement**: Inspect a provider before leasing
* **Monitoring**: Track a provider's attributes

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
