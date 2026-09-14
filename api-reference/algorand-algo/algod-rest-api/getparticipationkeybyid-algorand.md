---
description: >-
  Example code for the GetParticipationKeyByID REST method. Complete guide on
  how to use GetParticipationKeyByID REST in GetBlock Web3 documentation.
---

# GetParticipationKeyByID - Algorand

Given a participation ID, return information about that participation key.

{% hint style="warning" %}
This is a node-administration, participation, or diagnostic endpoint and is not served on GetBlock shared endpoints. It requires a Dedicated Node.
{% endhint %}

## Endpoint

```http
GET /v2/participation/{participation-id}
```

## Path Parameters

| Parameter        | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| participation-id | string | Yes      | —           |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}v2/participation/REPLACE_PARTICIPATION-ID"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field                 | Type    | Description                                                                               |
| --------------------- | ------- | ----------------------------------------------------------------------------------------- |
| address               | string  | Address the key was generated for.                                                        |
| effective-first-valid | integer | When registered, this is the first round it may be used.                                  |
| effective-last-valid  | integer | When registered, this is the last round it may be used.                                   |
| id                    | string  | The key's ParticipationID.                                                                |
| key                   | object  | AccountParticipation describes the parameters used by this account in consensus protocol. |
| last-block-proposal   | integer | Round when this key was last used to propose a block.                                     |
| last-state-proof      | integer | Round when this key was last used to generate a state proof.                              |
| last-vote             | integer | Round when this key was last used to vote.                                                |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
