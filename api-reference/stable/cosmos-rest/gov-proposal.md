---
description: >-
  Example code for the gov proposal REST method. Complete guide on how to use
  gov proposal REST method in GetBlock Web3 documentation.
---

# gov proposal - Stable

Returns a single governance proposal by its numeric identifier.

## Endpoint

```http
GET /cosmos/gov/v1/proposals/{proposal_id}
```

## Path Parameters

| Parameter    | Type   | Description                       |
| ------------ | ------ | --------------------------------- |
| proposal\_id | string | The proposal's numeric identifier |

## Example

{% code overflow="wrap" %}
```bash
export STABLE_REST=https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/

curl "${STABLE_REST}cosmos/gov/v1/proposals/900"
```
{% endcode %}

## Response

```json
{
    "proposal": {
        "id": "900",
        "status": "PROPOSAL_STATUS_VOTING_PERIOD",
        "messages": [],
        "final_tally_result": {
            "yes_count": "120000000000",
            "no_count": "5000000000",
            "abstain_count": "0",
            "no_with_veto_count": "0"
        },
        "voting_end_time": "2026-06-03T00:00:00Z"
    }
}
```

## Response Fields

| Field                         | Type   | Description               |
| ----------------------------- | ------ | ------------------------- |
| proposal.id                   | string | The proposal identifier   |
| proposal.status               | string | Proposal lifecycle status |
| proposal.final\_tally\_result | object | Current vote tally        |

## Use Cases

* **Proposal detail**: Render one proposal's text and tally

## Error Handling

| Error                     | Message       | Description                                                  |
| ------------------------- | ------------- | ------------------------------------------------------------ |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                 |
| 404 / Not found           | Not found     | The account, validator, proposal, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect            |
