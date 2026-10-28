---
description: >-
  Example code for the mint inflation REST method. Complete guide on how to use
  the mint inflation REST method in GetBlock Web3 documentation.
---

# mint inflation - Cosmos

Returns the current minting inflation rate.

## Endpoint

```
GET /cosmos/mint/v1beta1/inflation
```

## Example

{% code overflow="wrap" %}
```bash
export COSMOS_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${COSMOS_REST}cosmos/mint/v1beta1/inflation"
```
{% endcode %}

## Response

```json
{
    "inflation": "0.100000000000000000"
}
```

## Response Fields

| Field     | Type   | Description                                       |
| --------- | ------ | ------------------------------------------------- |
| inflation | string | Current annual inflation rate as a decimal string |

## Use Cases

* **Staking APR**: Combine inflation with the bonded ratio to estimate rewards
* **Metrics**: Track monetary policy over time

## Error Handling

| Error                     | Message       | Description                                                  |
| ------------------------- | ------------- | ------------------------------------------------------------ |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                 |
| 404 / Not found           | Not found     | The account, validator, proposal, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect            |
