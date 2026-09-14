---
description: >-
  Example code for the SimulateTransaction REST method. Complete guide on how to
  use SimulateTransaction REST in GetBlock Web3 documentation.
---

# SimulateTransaction - Algorand

Simulates a raw transaction or transaction group as it would be evaluated on the network. The simulation will use blockchain state from the latest committed round.

## Endpoint

```http
POST /v2/transactions/simulate
```

## Query Parameters

| Parameter | Type   | Required | Description                                                                                               |
| --------- | ------ | -------- | --------------------------------------------------------------------------------------------------------- |
| format    | string | Optional | Configures whether the response object is JSON or MessagePack encoded. If not provided, defaults to JSON. |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl --location --request POST "${ALGO_ALGOD}v2/transactions/simulate" \
--header 'Content-Type: application/json' \
--data-binary '<request body>'
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field             | Type    | Description                                                                                                                                                      |
| ----------------- | ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| eval-overrides    | object  | The set of parameters and limits override during simulation. If this set of parameters is present, then evaluation parameters may differ from standard evaluatio |
| exec-trace-config | object  | An object that configures simulation execution trace.                                                                                                            |
| initial-states    | object  | Initial states of resources that were accessed during simulation.                                                                                                |
| last-round        | integer | The round immediately preceding this simulation. State changes through this round were used to run this simulation.                                              |
| txn-groups        | array   | A result object for each transaction group that was simulated.                                                                                                   |
| version           | integer | The version of this response object.                                                                                                                             |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
