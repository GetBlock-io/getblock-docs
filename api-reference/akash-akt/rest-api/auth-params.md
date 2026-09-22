---
description: >-
  Example code for the cosmos/auth/v1beta1/params REST method. Complete guide
  on how to use cosmos/auth/v1beta1/params REST method in GetBlock Web3
  documentation.
---

# /cosmos/auth/v1beta1/params - Akash

Returns the auth module parameters, such as max memo characters and signature limits.

## Endpoint

```
GET /cosmos/auth/v1beta1/params
```

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}cosmos/auth/v1beta1/params"
```
{% endcode %}

## Response

```json
{
    "params": {
        "max_memo_characters": "256",
        "tx_sig_limit": "7",
        "sig_verify_cost_ed25519": "590"
    }
}
```

## Response Fields

| Field  | Type   | Description     |
| ------ | ------ | --------------- |
| params | object | Auth parameters |

## Use Cases

* **Validation**: Read tx size and signature limits

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
