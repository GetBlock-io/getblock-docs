---
description: >-
  Example code for the cosmos/base/tendermint/v1beta1/blocks/latest REST method.
  Complete guide on how to use cosmos/base/tendermint/v1beta1/blocks/latest REST
  method in GetBlock Web3 documentation.
---

# /cosmos/base/tendermint/v1beta1/blocks/latest - Cronos

Returns the latest committed block via the Cosmos base-tendermint service, with the block id, header, and transaction data in Cosmos-native encoding.

## Endpoint

```http
GET /cosmos/base/tendermint/v1beta1/blocks/latest
```

## Example

{% code overflow="wrap" %}
```bash
export CRONOS_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${CRONOS_REST}cosmos/base/tendermint/v1beta1/blocks/latest"
```
{% endcode %}

## Response

```json
{
    "block_id": {
        "hash": "b64=="
    },
    "block": {
        "header": {
            "chain_id": "cronosmainnet_25-1",
            "height": "12345678",
            "time": "2025-11-01T12:00:00Z",
            "proposer_address": "b64=="
        },
        "data": {
            "txs": [
                "Cr0BC..."
            ]
        }
    }
}
```

## Response Fields

| Field                  | Type   | Description                      |
| ---------------------- | ------ | -------------------------------- |
| block.header.height    | string | Latest block height              |
| block.header.chain\_id | string | Chain id                         |
| block.data.txs         | array  | Base64 transactions in the block |

## Use Cases

* **Chain Tip**: Read the latest height from the Cosmos gateway
* **Following**: Poll the latest block
* **Explorers**: Render the latest block

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| 500 / internal            | Internal error | The node failed to return the latest block        |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
