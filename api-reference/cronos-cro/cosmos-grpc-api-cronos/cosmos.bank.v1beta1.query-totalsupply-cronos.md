# cosmos.bank.v1beta1.Query/TotalSupply- Cronos

Returns the total supply of every denom, paginated. The gRPC equivalent of the REST supply query.

## Service Method

```
cosmos.bank.v1beta1.Query/TotalSupply
```

## Message Fields

| Field      | Type   | Description                      |
| ---------- | ------ | -------------------------------- |
| pagination | object | Optional pagination (limit, key) |

## Example

{% code overflow="wrap" %}
```bash
grpcurl \
  -H "x-api-key: <ACCESS-TOKEN>" \
  -d '{}' \
  shared.eu-central-1.getblock.io:443 \
  cosmos.bank.v1beta1.Query/TotalSupply
```
{% endcode %}

## Response

```json
{
    "supply": [
        {
            "denom": "basecro",
            "amount": "30000000000000000000000000000"
        }
    ],
    "pagination": {
        "total": "1"
    }
}
```

## Response Fields

| Field  | Type  | Description            |
| ------ | ----- | ---------------------- |
| supply | array | Total supply per denom |

## Use Cases

* **Tokenomics**: Read total supply
* **Analytics**: Track supply over time
* **Backends**: Typed supply reads

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| Internal                  | Internal error | The node failed to return supply                  |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
