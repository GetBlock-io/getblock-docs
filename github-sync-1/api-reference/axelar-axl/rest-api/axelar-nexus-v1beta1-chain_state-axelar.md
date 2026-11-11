---
description: >-
  Example code for the axelar/nexus/v1beta1/chain_state REST method. Complete
  guide on how to use axelar/nexus/v1beta1/chain_state REST method in GetBlock
  Web3 documentation.
---

# /axelar/nexus/v1beta1/chain\_state - Axelar

Returns the state of a registered chain: whether it is activated, its maintainers (validators securing it), and the assets registered on it.

## Endpoint

```http
GET /axelar/nexus/v1beta1/chain_state
```

## Query Parameters

| Parameter | Type   | Description                |
| --------- | ------ | -------------------------- |
| chain     | string | Chain name (e.g. ethereum) |

## Example

{% code overflow="wrap" %}
```bash
export AXELAR_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AXELAR_REST}axelar/nexus/v1beta1/chain_state?chain=ethereum"
```
{% endcode %}

## Response

```json
{
    "state": {
        "chain": {
            "name": "Ethereum",
            "supports_foreign_assets": true,
            "key_type": "KEY_TYPE_MULTISIG",
            "module": "evm"
        },
        "activated": true,
        "assets": [
            {
                "denom": "uaxl",
                "is_native_asset": false
            }
        ],
        "maintainer_states": []
    }
}
```

## Response Fields

| Field                    | Type    | Description                      |
| ------------------------ | ------- | -------------------------------- |
| state.activated          | boolean | Whether the chain is active      |
| state.assets             | array   | Assets registered on the chain   |
| state.maintainer\_states | array   | Validators maintaining the chain |

## Use Cases

* **Routing**: Confirm a chain is activated
* **Asset Support**: List assets on a chain

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
