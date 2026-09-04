# /cosmos/bank/v1beta1/balances/{address} - Cronos

Returns all coin balances held by a bech32 address, each as a denom and amount. Native CRO is reported in its base denom.

## Endpoint

```
GET /cosmos/bank/v1beta1/balances/{address}
```

## Path Parameters

| Parameter | Type   | Description                    |
| --------- | ------ | ------------------------------ |
| address   | string | Bech32 account address (crc1…) |

## Query Parameters

| Parameter        | Type   | Description           |
| ---------------- | ------ | --------------------- |
| pagination.limit | string | Max results to return |
| pagination.key   | string | Opaque next-page key  |

## Example

{% code overflow="wrap" %}
```bash
export CRONOS_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${CRONOS_REST}cosmos/bank/v1beta1/balances/crc1p9zq0lr6h6n5s2wk9m3v7t4x8c6b2d0f1g3h5j"
```
{% endcode %}

## Response

```json
{
    "balances": [
        {
            "denom": "basecro",
            "amount": "1000000000000000000"
        }
    ],
    "pagination": {
        "next_key": null,
        "total": "1"
    }
}
```

## Response Fields

| Field                | Type   | Description                      |
| -------------------- | ------ | -------------------------------- |
| balances             | array  | Coin balances as {denom, amount} |
| pagination.next\_key | string | null                             |

## Use Cases

* **Balance Reads**: Display an address's token holdings
* **Portfolio**: Enumerate every denom an account holds
* **Accounting**: Reconcile balances off-chain

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| 400 / invalid address     | Invalid address | The address is not valid bech32                   |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
