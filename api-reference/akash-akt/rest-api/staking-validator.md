---
description: >-
  Example code for the cosmos/staking/v1beta1/validators/{validator_addr} REST
  method. Complete guide on how to use
  cosmos/staking/v1beta1/validators/{validator_addr} REST method in GetBlock
  Web3 documentation.
---

# /cosmos/staking/v1beta1/validators/{validator\_addr} - Akash

Returns one staking validator by operator address.

## Endpoint

```http
GET /cosmos/staking/v1beta1/validators/{validator_addr}
```

## Path Parameters

| Parameter       | Type   | Description                       |
| --------------- | ------ | --------------------------------- |
| validator\_addr | string | Operator address (akashvaloper1…) |

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}cosmos/staking/v1beta1/validators/akashvaloper1qx9knjqah8s0l4rhwmapsd6cuah4948jy0gf0t"
```
{% endcode %}

## Response

```json
{
    "validator": {
        "operator_address": "akashvaloper1qx9knjqah8s0l4rhwmapsd6cuah4948jy0gf0t",
        "consensus_pubkey": {
            "@type": "/cosmos.crypto.ed25519.PubKey",
            "key": "Ka3CNR8sDNYlC0Y845p6k/usONTufK5vsL10kMUlTtM="
        },
        "jailed": false,
        "status": "BOND_STATUS_BONDED",
        "tokens": "399745659305",
        "delegator_shares": "399745659305.000000000000000000",
        "description": {
            "moniker": "cosmosrescue",
            "identity": "5489ADE7B1B91C65",
            "website": "https://cosmosrescue.com",
            "security_contact": "contact@cosmosrescue.com",
            "details": "\ud83d\udc7e Securing the Cosmos! Contributing to the community with Cosmobot, providing pu..."
        },
        "unbonding_height": "0",
        "unbonding_time": "1970-01-01T00:00:00Z",
        "commission": {
            "commission_rates": {
                "rate": "0.050000000000000000",
                "max_rate": "0.100000000000000000",
                "max_change_rate": "0.010000000000000000"
            },
            "update_time": "2024-05-10T14:16:35.609887686Z"
        },
        "min_self_delegation": "1",
        "unbonding_on_hold_ref_count": "0",
        "unbonding_ids": []
    }
}
```

## Response Fields

| Field     | Type   | Description       |
| --------- | ------ | ----------------- |
| validator | object | Validator details |

## Use Cases

* **Validator Pages**: Render a validator

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
