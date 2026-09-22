---
description: >-
  Example code for the akash/provider/v1beta4/providers/{owner} REST method.
  Complete guide on how to use akash/provider/v1beta4/providers/{owner} REST
  method in GetBlock Web3 documentation.
---

# /akash/provider/v1beta4/providers/{owner} - Akash

Returns a single provider by its owner address, with its host URI, attributes, and contact info.

## Endpoint

```http
GET /akash/provider/v1beta4/providers/{owner}
```

## Path Parameters

| Parameter | Type   | Description                      |
| --------- | ------ | -------------------------------- |
| owner     | string | Provider owner address (akash1…) |

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}akash/provider/v1beta4/providers/akash1qpy7waukp7nxlcxmwjcgtmyzu2auk3p5dc3gg6"
```
{% endcode %}

## Response

```json
{
    "provider": {
        "owner": "akash1qpy7waukp7nxlcxmwjcgtmyzu2auk3p5dc3gg6",
        "host_uri": "https://provider.forgeelectronics.uk:8443",
        "attributes": [
            {
                "key": "host",
                "value": "akash"
            }
        ],
        "info": {
            "email": "christianpenrod@gmail.com",
            "website": ""
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
