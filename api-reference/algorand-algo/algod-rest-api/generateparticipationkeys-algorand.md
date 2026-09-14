---
description: >-
  Example code for the GenerateParticipationKeys REST method. Complete guide on
  how to use GenerateParticipationKeys REST in GetBlock Web3 documentation.
---

# GenerateParticipationKeys - Algorand

Generate and install participation keys to the node.

{% hint style="warning" %}
This is a node-administration, participation, or diagnostic endpoint and is not served on GetBlock shared endpoints. It requires a Dedicated Node.
{% endhint %}

## Endpoint

```http
POST /v2/participation/generate/{address}
```

## Path Parameters

| Parameter | Type   | Required | Description            |
| --------- | ------ | -------- | ---------------------- |
| address   | string | Yes      | An account public key. |

## Query Parameters

| Parameter | Type    | Required | Description                                                                          |
| --------- | ------- | -------- | ------------------------------------------------------------------------------------ |
| dilution  | integer | Optional | Key dilution for two-level participation keys (defaults to sqrt of validity window). |
| first     | integer | Yes      | First round for participation key.                                                   |
| last      | integer | Yes      | Last round for participation key.                                                    |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl --location --request POST "${ALGO_ALGOD}v2/participation/generate/REPLACE_ADDRESS" \
--header 'Content-Type: application/json' \
--data-binary '<request body>'
```
{% endcode %}

## Response

Returns HTTP 200 on success.

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
