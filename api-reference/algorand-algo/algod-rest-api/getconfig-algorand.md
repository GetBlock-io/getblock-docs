---
description: >-
  Example code for the GetConfig REST method. Complete guide on how to use
  GetConfig REST in GetBlock Web3 documentation.
---

# GetConfig - Algorand

Returns the merged (defaults + overrides) config file in JSON.

{% hint style="warning" %}
This is a node-administration, participation, or diagnostic endpoint and is not served on GetBlock shared endpoints. It requires a Dedicated Node.
{% endhint %}

## Endpoint

```http
GET /debug/settings/config
```

## Example

```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}debug/settings/config"
```

## Response

Returns HTTP 200 on success.

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
