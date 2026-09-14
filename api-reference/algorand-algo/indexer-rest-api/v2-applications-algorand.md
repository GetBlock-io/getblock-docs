---
description: >-
  Example code for the /v2/applications REST method. Complete guide on how to
  use the /v2/applications REST method in the GetBlock Web3 documentation.
---

# /v2/applications - Algorand

Searches applications (smart contracts) by application ID or creator. Paginated.

## Endpoint

```http
GET /v2/applications
```

## Query Parameters

| Parameter      | Type    | Description                      |
| -------------- | ------- | -------------------------------- |
| application-id | integer | Filter to a specific application |
| creator        | string  | Filter by creator address        |
| limit          | integer | Max results                      |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_INDEXER=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_INDEXER}v2/applications"
```
{% endcode %}

## Response

```json
{
    "applications": [
        {
            "id": 350338509,
            "params": {
                "creator": "XBYLS2E6YI6XXL5BWCAMOA4GTWHXWENZMX5UHXMRNWWUQ7BXCY5WC5TEPA",
                "global-state-schema": {
                    "num-uint": 1,
                    "num-byte-slice": 0
                }
            }
        }
    ],
    "current-round": 35000000,
    "next-token": "b..."
}
```

## Response Fields

| Field        | Type   | Description                           |
| ------------ | ------ | ------------------------------------- |
| applications | array  | Matching applications with parameters |
| next-token   | string | Pagination token                      |

## Use Cases

* **dApp Discovery**: Find applications by creator
* **Explorers**: Power an application search
* **Analytics**: Enumerate smart contracts

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A filter parameter is invalid                     |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
