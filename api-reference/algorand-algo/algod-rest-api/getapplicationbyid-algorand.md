---
description: >-
  Example code for the GetApplicationByID REST method. Complete guide on how to
  use GetApplicationByID REST in GetBlock Web3 documentation.
---

# GetApplicationByID - Algorand

Given an application ID, returns application information including creator, approval and clear programs, global and local schemas, and global state.

## Endpoint

```http
GET /v2/applications/{application-id}
```

## Path Parameters

| Parameter      | Type    | Required | Description                |
| -------------- | ------- | -------- | -------------------------- |
| application-id | integer | Yes      | An application identifier. |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}v2/applications/REPLACE_APPLICATION-ID"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field  | Type    | Description                                                   |
| ------ | ------- | ------------------------------------------------------------- |
| id     | integer | \[appidx] application index.                                  |
| params | object  | Stores the global information associated with an application. |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
