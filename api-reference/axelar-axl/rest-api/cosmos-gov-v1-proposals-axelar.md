---
description: >-
  Example code for the cosmos/gov/v1/proposals REST method. Complete guide on
  how to use cosmos/gov/v1/proposals REST method in GetBlock Web3 documentation.
---

# /cosmos/gov/v1/proposals - Axelar

Returns the paginated list of governance proposals, each with its id, status, and tally.

## Endpoint

```
GET /cosmos/gov/v1/proposals
```

## Query Parameters

| Parameter        | Type   | Description      |
| ---------------- | ------ | ---------------- |
| proposal\_status | string | Filter by status |
| pagination.limit | string | Max results      |

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}cosmos/gov/v1/proposals"
```
{% endcode %}

## Response

```json
{
    "proposals": [
        {
            "id": "42",
            "status": "PROPOSAL_STATUS_VOTING_PERIOD",
            "final_tally_result": {
                "yes_count": "0",
                "no_count": "0"
            }
        }
    ],
    "pagination": {
        "total": "42"
    }
}
```

## Response Fields

| Field     | Type  | Description                                 |
| --------- | ----- | ------------------------------------------- |
| proposals | array | Governance proposals with id, status, tally |

## Use Cases

* **Governance UIs**: List proposals
* **Voting**: Surface active proposals
* **Monitoring**: Alert on new proposals

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
