---
description: >-
  Example code for the AddParticipationKey REST method. Complete guide on how to
  use AddParticipationKey REST in GetBlock Web3 documentation.
---

# AddParticipationKey - Algorand

Add a participation key to the node.

{% hint style="warning" %}
This is a node-administration, participation, or diagnostic endpoint and is not served on GetBlock shared endpoints. It requires a Dedicated Node.
{% endhint %}

## Endpoint

```http
POST /v2/participation
```

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl --location --request POST "${ALGO_ALGOD}v2/participation" \
--header 'Content-Type: application/msgpack' \
--data-binary '<request body>'
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field  | Type   | Description                       |
| ------ | ------ | --------------------------------- |
| partId | string | encoding of the participation ID. |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
