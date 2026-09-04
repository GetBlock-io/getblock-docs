---
description: >-
  Example code for the cosmos.gov.v1.Query/Proposals gRPC method. Complete guide
  on how to use cosmos.gov.v1.Query/Proposals gRPC method in GetBlock Web3
  documentation.
---

# cosmos.gov.v1.Query/Proposals - Cronos

Returns the paginated list of governance proposals, optionally filtered by status, voter, or depositor. The gRPC equivalent of the REST proposals query.

## Service Method

```
cosmos.gov.v1.Query/Proposals
```

## Message Fields

| Field            | Type   | Description            |
| ---------------- | ------ | ---------------------- |
| proposal\_status | string | Optional status filter |
| pagination       | object | Optional pagination    |

## Example

{% code overflow="wrap" %}
```bash
grpcurl \
  -H "x-api-key: <ACCESS-TOKEN>" \
  -d '{
    "proposal_status": "PROPOSAL_STATUS_VOTING_PERIOD"
}' \
  shared.eu-central-1.getblock.io:443 \
  cosmos.gov.v1.Query/Proposals
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

| Field     | Type  | Description                                     |
| --------- | ----- | ----------------------------------------------- |
| proposals | array | Governance proposals with id, status, and tally |

## Use Cases

* **Governance UIs**: List proposals
* **Backends**: Typed governance reads
* **Monitoring**: Watch for new proposals

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| Internal                  | Internal error | The node failed to return proposals               |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
