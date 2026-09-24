---
description: >-
  Example code for the axelar/reward/v1beta1/params REST method. Complete guide
  on how to use axelar/reward/v1beta1/params REST method in GetBlock Web3
  documentation.
---

# /axelar/reward/v1beta1/params - Axelar

Returns the parameters of Axelar's reward module, which pays validators for external-chain voting and key management.

## Endpoint

```http
GET /axelar/reward/v1beta1/params
```

## Example

{% code overflow="wrap" %}
```bash
export AXELAR_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AXELAR_REST}axelar/reward/v1beta1/params"
```
{% endcode %}

## Response

```json
{
    "params": {
        "external_chain_voting_inflation_rate": "0.02",
        "key_mgmt_relative_inflation_rate": "2.0",
        "tss_relative_inflation_rate": "1.0"
    }
}
```

## Response Fields

| Field  | Type   | Description                      |
| ------ | ------ | -------------------------------- |
| params | object | Reward inflation-rate parameters |

## Use Cases

* **Tokenomics**: Read validator reward configuration
* **Analytics**: Model validator incentives

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
