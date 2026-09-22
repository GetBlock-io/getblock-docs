# latest block

Returns the latest committed block via the Cosmos base-tendermint service, with the block id, header, and transaction data.

## Endpoint

```http
GET /cosmos/base/tendermint/v1beta1/blocks/latest
```

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}cosmos/base/tendermint/v1beta1/blocks/latest"
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
            "chain_id": "akashnet-2",
            "height": "19500000",
            "time": "2025-11-01T12:00:00Z"
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

| Field                  | Type   | Description         |
| ---------------------- | ------ | ------------------- |
| block.header.height    | string | Latest block height |
| block.header.chain\_id | string | Chain id            |

## Use Cases

* **Chain Tip**: Read the latest height
* **Following**: Poll the latest block
* **Explorers**: Render the latest block

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
