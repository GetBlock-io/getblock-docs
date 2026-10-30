---
description: >-
  Example code for the gov proposals REST method. Complete guide on how to use
  gov proposals REST method in GetBlock Web3 documentation.
---

# gov proposals

Returns governance proposals, optionally filtered by status and paginated.

## Endpoint

```http
GET /cosmos/gov/v1/proposals
```

## Query Parameters

| Parameter        | Type   | Description                                             |
| ---------------- | ------ | ------------------------------------------------------- |
| proposal\_status | string | Filter by status, e.g. PROPOSAL\_STATUS\_VOTING\_PERIOD |
| pagination.limit | string | Max results per page                                    |

## Example

{% code overflow="wrap" %}
```bash
export STABLE_REST=https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/

curl "${STABLE_REST}cosmos/gov/v1/proposals"
```
{% endcode %}

## Response

```json
{
    "proposals": [
        {
            "id": "900",
            "status": "PROPOSAL_STATUS_VOTING_PERIOD",
            "messages": [],
            "final_tally_result": {
                "yes_count": "0",
                "no_count": "0"
            },
            "submit_time": "2026-05-20T00:00:00Z",
            "voting_end_time": "2026-06-03T00:00:00Z"
        }
    ],
    "pagination": {
        "total": "900"
    }
}
```

## Response Fields

| Field                             | Type   | Description                                             |
| --------------------------------- | ------ | ------------------------------------------------------- |
| proposals                         | array  | Governance proposals with status, messages, and tallies |
| proposals\[].status               | string | Proposal lifecycle status                               |
| proposals\[].final\_tally\_result | object | Vote tally for the proposal                             |

## Use Cases

* **Governance UI**: List active and past proposals
* **Monitoring**: Track proposals in the voting period

## Error Handling

| Error                     | Message       | Description                                                  |
| ------------------------- | ------------- | ------------------------------------------------------------ |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                 |
| 404 / Not found           | Not found     | The account, validator, proposal, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect            |
