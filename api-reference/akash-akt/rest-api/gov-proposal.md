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

curl "${AKASH_REST}cosmos/gov/v1/proposals/341"
```
{% endcode %}

## Response

```json
{
    "proposal": {
        "id": "341",
        "messages": [
            {
                "@type": "/cosmos.distribution.v1beta1.MsgCommunityPoolSpend",
                "authority": "akash10d07y265gmmuvt4z0w9aw880jnsr700jhe7z0f",
                "recipient": "akash1nw9k9336g9csenjaq74f87gc4w4ffrv3wa3um0",
                "amount": [
                    {
                        "denom": "uakt",
                        "amount": "230557400000"
                    }
                ]
            }
        ],
        "status": "PROPOSAL_STATUS_VOTING_PERIOD",
        "final_tally_result": {
            "yes_count": "0",
            "abstain_count": "0",
            "no_count": "0",
            "no_with_veto_count": "0"
        },
        "submit_time": "2026-09-18T15:22:26.035897772Z",
        "deposit_end_time": "2026-10-02T15:22:26.035897772Z",
        "total_deposit": [
            {
                "denom": "uakt",
                "amount": "1000000000"
            }
        ],
        "voting_start_time": "2026-09-18T15:22:26.035897772Z",
        "voting_end_time": "2026-09-25T15:22:26.035897772Z",
        "metadata": "{\"title\":\"Hermes Price Relayer Operations \u2013 Foundation AKT Funding Request\",\"sum...",
        "title": "Hermes Price Relayer Operations \u2013 Foundation AKT Funding Request",
        "summary": "# **Hermes Price Relayer Operations \u2013 Foundation AKT Funding Request**\n\n**Summar...",
        "proposer": "akash1rf2g7shyy4chfa58xkmr504a2fjchydmsxsasq",
        "expedited": false,
        "failed_reason": ""
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
