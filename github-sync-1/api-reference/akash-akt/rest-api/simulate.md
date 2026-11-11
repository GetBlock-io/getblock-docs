---
description: >-
  Example code for the cosmos/tx/v1beta1/simulate REST method. Complete guide
  on how to use cosmos/tx/v1beta1/simulate REST method in GetBlock Web3
  documentation.
---

# /cosmos/tx/v1beta1/simulate - Akash

Simulates a signed transaction and returns the gas it would use and the resulting events, without broadcasting.

## Endpoint

```
POST /cosmos/tx/v1beta1/simulate
```

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl --location --request POST "${AKASH_REST}cosmos/tx/v1beta1/simulate" \
--header 'Content-Type: application/json' \
--data-raw '{
    "tx_bytes": "Cr0BC...",
    "tx": null
}'
```
{% endcode %}

## Response

```json
{
    "gas_info": {
        "gas_wanted": "200000",
        "gas_used": "118000"
    },
    "result": {
        "events": []
    }
}
```

## Response Fields

| Field     | Type   | Description                           |
| --------- | ------ | ------------------------------------- |
| gas\_info | object | Gas wanted/used from simulation       |
| result    | object | Simulated execution result and events |

## Use Cases

* **Fee Estimation**: Estimate gas before signing

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
