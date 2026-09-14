---
description: >-
  Example code for the /v2/ledger/supply REST method. Complete guide on how to
  use the /v2/ledger/supply REST method in the GetBlock Web3 documentation.
---

# /v2/ledger/supply - Algorand

Returns the current total and online (participating) ALGO supply reported by the ledger, along with the current round.

## Endpoint

```http
GET /v2/ledger/supply
```

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}v2/ledger/supply"
```
{% endcode %}

## Response

```json
{
    "current_round": 35000000,
    "total-money": 10000000000000000,
    "online-money": 2000000000000000
}
```

## Response Fields

| Field          | Type    | Description                           |
| -------------- | ------- | ------------------------------------- |
| total-money    | integer | Total ALGO supply in microAlgos       |
| online-money   | integer | Online (staking) supply in microAlgos |
| current\_round | integer | Round the supply was read at          |

## Use Cases

* **Tokenomics**: Read total and online supply
* **Staking Ratio**: Compute the participation ratio
* **Dashboards**: Show supply metrics

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| 500 / Internal            | Internal error | The node failed to return supply                  |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
