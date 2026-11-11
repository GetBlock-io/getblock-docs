---
description: >-
  Example code for the akash/inflation/v1beta3/params REST method. Complete
  guide on how to use akash/inflation/v1beta3/params REST method in GetBlock
  Web3 documentation.
---

# /akash/inflation/v1beta3/params - Akash

Returns Akash's inflation module parameters, which govern token issuance and the community/provider incentive split.

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
GET /akash/inflation/v1beta3/params
```

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}akash/inflation/v1beta3/params"
```
{% endcode %}

## Response

```json
{
    "params": {
        "inflation_decay_factor": "0.75",
        "initial_inflation": "100.0",
        "variance": "0.05"
    }
}
```

## Response Fields

| Field  | Type   | Description                |
| ------ | ------ | -------------------------- |
| params | object | Akash inflation parameters |

## Use Cases

* **Tokenomics**: Read Akash-specific inflation config

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
