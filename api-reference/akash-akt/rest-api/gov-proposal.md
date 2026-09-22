---
description: >-
  Example code for the cosmos/gov/v1/proposals/{proposal_id} REST method.
  Complete guide on how to use cosmos/gov/v1/proposals/{proposal_id} REST
  method in GetBlock Web3 documentation.
---

# /cosmos/gov/v1/proposals/{proposal\_id} - Akash

Returns one governance proposal by id, with its messages, status, and tally.

## Endpoint

```http
GET /cosmos/gov/v1/proposals/{proposal_id}
```

## Path Parameters

| Parameter    | Type   | Description |
| ------------ | ------ | ----------- |
| proposal\_id | string | Proposal id |

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}cosmos/gov/v1/proposals/42"
```
{% endcode %}

## Response

```json
{
    "proposal": {
        "id": "42",
        "status": "PROPOSAL_STATUS_VOTING_PERIOD",
        "final_tally_result": {
            "yes_count": "0"
        },
        "voting_end_time": "2025-11-15T00:00:00Z"
    }
}
```

## Response Fields

| Field    | Type   | Description     |
| -------- | ------ | --------------- |
| proposal | object | Proposal detail |

## Use Cases

* **Governance**: Render a proposal page

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
