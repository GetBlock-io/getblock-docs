---
description: >-
  Example code for the cosmos/gov/v1/proposals/{proposal_id}/tally REST
  method. Complete guide on how to use
  cosmos/gov/v1/proposals/{proposal_id}/tally REST method in GetBlock Web3
  documentation.
---

# /cosmos/gov/v1/proposals/{proposal\_id}/tally - Akash

Returns the current tally of votes for a proposal.

## Endpoint

```http
GET /cosmos/gov/v1/proposals/{proposal_id}/tally
```

## Path Parameters

| Parameter    | Type   | Description |
| ------------ | ------ | ----------- |
| proposal\_id | string | Proposal id |

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}cosmos/gov/v1/proposals/341/tally"
```
{% endcode %}

## Response

```json
{
    "tally": {
        "yes_count": "35102265275414",
        "abstain_count": "763068307757",
        "no_count": "8545777331",
        "no_with_veto_count": "0"
    }
}
```

## Response Fields

| Field | Type   | Description         |
| ----- | ------ | ------------------- |
| tally | object | Current vote counts |

## Use Cases

* **Voting**: Show live tally

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
