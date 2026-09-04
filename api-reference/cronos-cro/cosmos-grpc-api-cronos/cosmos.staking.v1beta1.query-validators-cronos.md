# cosmos.staking.v1beta1.Query/Validators - Cronos

Returns the paginated set of staking validators, optionally filtered by bond status. The gRPC equivalent of the REST validators query.

## Service Method

```
cosmos.staking.v1beta1.Query/Validators
```

## Message Fields

| Field      | Type   | Description                 |
| ---------- | ------ | --------------------------- |
| status     | string | Optional bond-status filter |
| pagination | object | Optional pagination         |

## Example

{% code overflow="wrap" %}
```bash
grpcurl \
  -H "x-api-key: <ACCESS-TOKEN>" \
  -d '{
    "status": "BOND_STATUS_BONDED"
}' \
  shared.eu-central-1.getblock.io:443 \
  cosmos.staking.v1beta1.Query/Validators
```
{% endcode %}

## Response

```json
{
    "validators": [
        {
            "operator_address": "crcvaloper1p9zq0lr6h6n5s2wk9m3v7t4x8c6b2d0f1g3h5j",
            "status": "BOND_STATUS_BONDED",
            "tokens": "5000000000000000000000000",
            "commission": {
                "commission_rates": {
                    "rate": "0.100000000000000000"
                }
            }
        }
    ],
    "pagination": {
        "total": "30"
    }
}
```

## Response Fields

| Field      | Type   | Description                                    |
| ---------- | ------ | ---------------------------------------------- |
| validators | array  | Validator operators with tokens and commission |
| pagination | object | Pagination cursor                              |

## Use Cases

* **Staking UIs**: List validators for delegation
* **Backends**: Typed validator reads
* **Analytics**: Aggregate bonded stake

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| Internal                  | Internal error | The node failed to return validators              |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
