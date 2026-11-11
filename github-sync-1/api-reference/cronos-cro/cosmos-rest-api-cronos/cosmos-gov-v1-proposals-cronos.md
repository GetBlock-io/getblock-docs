---
description: >-
  Example code for the cosmos/gov/v1/proposals REST method. Complete guide on
  how to use cosmos/gov/v1/proposals REST method in GetBlock Web3 documentation.
---

# /cosmos/gov/v1/proposals - Cronos

Returns the paginated list of governance proposals, each with its id, status, messages, and tally. Filter by status with a query parameter.

## Endpoint

```http
GET /cosmos/gov/v1/proposals
```

## Query Parameters

| Parameter        | Type   | Description                             |
| ---------------- | ------ | --------------------------------------- |
| proposal\_status | string | Filter by voting/passed/rejected status |
| pagination.limit | string | Max results to return                   |

## Example

{% code overflow="wrap" %}
```bash
export CRONOS_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${CRONOS_REST}cosmos/gov/v1/proposals"
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
            },
            "submit_time": "2025-10-01T00:00:00Z",
            "voting_end_time": "2025-10-15T00:00:00Z"
        }
    ],
    "pagination": {
        "total": "42"
    }
}
```

## Response Fields

| Field                             | Type   | Description                                     |
| --------------------------------- | ------ | ----------------------------------------------- |
| proposals                         | array  | Governance proposals with id, status, and tally |
| proposals\[].status               | string | Proposal lifecycle status                       |
| proposals\[].final\_tally\_result | object | Vote counts (yes/no/abstain/veto)               |

## Use Cases

* **Governance UIs**: List active and past proposals
* **Voting**: Surface proposals in the voting period
* **Monitoring**: Alert on new proposals

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| 500 / internal            | Internal error | The node failed to return proposals               |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
