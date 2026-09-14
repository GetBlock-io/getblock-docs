---
description: >-
  Example code for the GetDebugSettingsProf REST method. Complete guide on how
  to use GetDebugSettingsProf REST in GetBlock Web3 documentation.
---

# GetDebugSettingsProf - Algorand

Retrieves the current settings for blocking and mutex profiles.

{% hint style="warning" %}
This is a node-administration, participation, or diagnostic endpoint and is not served on GetBlock shared endpoints. It requires a Dedicated Node.
{% endhint %}

## Endpoint

```http
GET /debug/settings/pprof
```

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}debug/settings/pprof"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field      | Type    | Description                                                                                                                                                      |
| ---------- | ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| block-rate | integer | The rate of blocking events. The profiler aims to sample an average of one blocking event per rate nanoseconds spent blocked. To turn off profiling entirely, pa |
| mutex-rate | integer | The rate of mutex events. On average 1/rate events are reported. To turn off profiling entirely, pass rate 0                                                     |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
