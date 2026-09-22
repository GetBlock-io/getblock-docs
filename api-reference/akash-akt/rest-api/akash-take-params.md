---
description: >-
  Example code for the akash/take/v1beta3/params REST method. Complete guide
  on how to use akash/take/v1beta3/params REST method in GetBlock Web3
  documentation.
---

# /akash/take/v1beta3/params - Akash

Returns the marketplace take parameters — the protocol fee taken from lease payments, per denom.

{% hint style="danger" %}
**This endpoint is not available on GetBlock's Akash REST endpoint.** Every request returns `501 Not Implemented`:

```json
{
    "jsonrpc": "",
    "error": {
        "code": -32701,
        "message": "not implemented"
    }
}
```

The gateway returns that error for any path it does not route, and no module version resolves it: `v1`, `v1beta1` through `v1beta5` were all tried. The other Akash modules — deployment, market, provider, cert, and audit — do respond, so this is specific to the module below rather than to Akash paths in general.
{% endhint %}

## Endpoint

```http
GET /akash/take/v1beta3/params
```

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}akash/take/v1beta3/params"
```
{% endcode %}

## Response

```json
{
    "params": {
        "default_take_rate": 20,
        "denom_take_rates": [
            {
                "denom": "uakt",
                "rate": 20
            }
        ]
    }
}
```

## Response Fields

| Field  | Type   | Description                       |
| ------ | ------ | --------------------------------- |
| params | object | Take-rate configuration per denom |

## Use Cases

* **Fees**: Read the marketplace fee
* **Economics**: Model provider net revenue

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
