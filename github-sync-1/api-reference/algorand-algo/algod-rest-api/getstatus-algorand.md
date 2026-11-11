---
description: >-
  Example code for the GetStatus REST method. Complete guide on how to use
  GetStatus REST in GetBlock Web3 documentation.
---

# GetStatus - Algorand

Gets the current node status.

## Endpoint

```http
GET /v2/status
```

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}v2/status"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field                             | Type    | Description                                                                                                       |
| --------------------------------- | ------- | ----------------------------------------------------------------------------------------------------------------- |
| catchpoint                        | string  | The current catchpoint that is being caught up to                                                                 |
| catchpoint-acquired-blocks        | integer | The number of blocks that have already been obtained by the node as part of the catchup                           |
| catchpoint-processed-accounts     | integer | The number of accounts from the current catchpoint that have been processed so far as part of the catchup         |
| catchpoint-processed-kvs          | integer | The number of key-values (KVs) from the current catchpoint that have been processed so far as part of the catchup |
| catchpoint-total-accounts         | integer | The total number of accounts included in the current catchpoint                                                   |
| catchpoint-total-blocks           | integer | The total number of blocks that are required to complete the current catchpoint catchup                           |
| catchpoint-total-kvs              | integer | The total number of key-values (KVs) included in the current catchpoint                                           |
| catchpoint-verified-accounts      | integer | The number of accounts from the current catchpoint that have been verified so far as part of the catchup          |
| catchpoint-verified-kvs           | integer | The number of key-values (KVs) from the current catchpoint that have been verified so far as part of the catchup  |
| catchup-time                      | integer | CatchupTime in nanoseconds                                                                                        |
| last-catchpoint                   | string  | The last catchpoint seen by the node                                                                              |
| last-round                        | integer | LastRound indicates the last round seen                                                                           |
| last-version                      | string  | LastVersion indicates the last consensus version supported                                                        |
| next-version                      | string  | NextVersion of consensus protocol to use                                                                          |
| next-version-round                | integer | NextVersionRound is the round at which the next consensus version will apply                                      |
| next-version-supported            | boolean | NextVersionSupported indicates whether the next consensus version is supported by this node                       |
| stopped-at-unsupported-round      | boolean | StoppedAtUnsupportedRound indicates that the node does not support the new rounds and has stopped making progress |
| time-since-last-round             | integer | TimeSinceLastRound in nanoseconds                                                                                 |
| upgrade-delay                     | integer | Upgrade delay                                                                                                     |
| upgrade-next-protocol-vote-before | integer | Next protocol round                                                                                               |
| upgrade-no-votes                  | integer | No votes cast for consensus upgrade                                                                               |
| upgrade-node-vote                 | boolean | This node's upgrade vote                                                                                          |
| upgrade-vote-rounds               | integer | Total voting rounds for current upgrade                                                                           |
| upgrade-votes                     | integer | Total votes cast for consensus upgrade                                                                            |
| upgrade-votes-required            | integer | Yes votes required for consensus upgrade                                                                          |
| upgrade-yes-votes                 | integer | Yes votes cast for consensus upgrade                                                                              |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
