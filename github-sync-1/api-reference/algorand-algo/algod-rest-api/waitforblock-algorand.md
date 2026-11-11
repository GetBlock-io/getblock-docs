---
description: >-
  Example code for the WaitForBlock REST method. Complete guide on how to use
  WaitForBlock REST in GetBlock Web3 documentation.
---

# WaitForBlock - Algorand

Waits for a block to appear after round `{round}` and returns the node's status at the time. There is a 1 minute timeout, when reached the current status is returned regardless of whether or not it is the round after the given round.

## Endpoint

```http
GET /v2/status/wait-for-block-after/{round}
```

## Path Parameters

| Parameter | Type    | Required | Description     |
| --------- | ------- | -------- | --------------- |
| round     | integer | Yes      | A round number. |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}v2/status/wait-for-block-after/REPLACE_ROUND"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

<table data-search="false"><thead><tr><th>Field</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td>catchpoint</td><td>string</td><td>The current catchpoint that is being caught up to</td></tr><tr><td>catchpoint-acquired-blocks</td><td>integer</td><td>The number of blocks that have already been obtained by the node as part of the catchup</td></tr><tr><td>catchpoint-processed-accounts</td><td>integer</td><td>The number of accounts from the current catchpoint that have been processed so far as part of the catchup</td></tr><tr><td>catchpoint-processed-kvs</td><td>integer</td><td>The number of key-values (KVs) from the current catchpoint that have been processed so far as part of the catchup</td></tr><tr><td>catchpoint-total-accounts</td><td>integer</td><td>The total number of accounts included in the current catchpoint</td></tr><tr><td>catchpoint-total-blocks</td><td>integer</td><td>The total number of blocks that are required to complete the current catchpoint catchup</td></tr><tr><td>catchpoint-total-kvs</td><td>integer</td><td>The total number of key-values (KVs) included in the current catchpoint</td></tr><tr><td>catchpoint-verified-accounts</td><td>integer</td><td>The number of accounts from the current catchpoint that have been verified so far as part of the catchup</td></tr><tr><td>catchpoint-verified-kvs</td><td>integer</td><td>The number of key-values (KVs) from the current catchpoint that have been verified so far as part of the catchup</td></tr><tr><td>catchup-time</td><td>integer</td><td>CatchupTime in nanoseconds</td></tr><tr><td>last-catchpoint</td><td>string</td><td>The last catchpoint seen by the node</td></tr><tr><td>last-round</td><td>integer</td><td>LastRound indicates the last round seen</td></tr><tr><td>last-version</td><td>string</td><td>LastVersion indicates the last consensus version supported</td></tr><tr><td>next-version</td><td>string</td><td>NextVersion of consensus protocol to use</td></tr><tr><td>next-version-round</td><td>integer</td><td>NextVersionRound is the round at which the next consensus version will apply</td></tr><tr><td>next-version-supported</td><td>boolean</td><td>NextVersionSupported indicates whether the next consensus version is supported by this node</td></tr><tr><td>stopped-at-unsupported-round</td><td>boolean</td><td>StoppedAtUnsupportedRound indicates that the node does not support the new rounds and has stopped making progress</td></tr><tr><td>time-since-last-round</td><td>integer</td><td>TimeSinceLastRound in nanoseconds</td></tr><tr><td>upgrade-delay</td><td>integer</td><td>Upgrade delay</td></tr><tr><td>upgrade-next-protocol-vote-before</td><td>integer</td><td>Next protocol round</td></tr><tr><td>upgrade-no-votes</td><td>integer</td><td>No votes cast for consensus upgrade</td></tr><tr><td>upgrade-node-vote</td><td>boolean</td><td>This node's upgrade vote</td></tr><tr><td>upgrade-vote-rounds</td><td>integer</td><td>Total voting rounds for current upgrade</td></tr><tr><td>upgrade-votes</td><td>integer</td><td>Total votes cast for consensus upgrade</td></tr><tr><td>upgrade-votes-required</td><td>integer</td><td>Yes votes required for consensus upgrade</td></tr><tr><td>upgrade-yes-votes</td><td>integer</td><td>Yes votes cast for consensus upgrade</td></tr></tbody></table>

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
