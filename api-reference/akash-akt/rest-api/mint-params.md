# mint params

Returns the mint module parameters (inflation bounds, goal bonded, blocks per year).

## Endpoint

```
GET /cosmos/mint/v1beta1/params
```

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}cosmos/mint/v1beta1/params"
```
{% endcode %}

## Response

```json
{
    "params": {
        "mint_denom": "uakt",
        "inflation_max": "0.13",
        "inflation_min": "0.01",
        "goal_bonded": "0.67",
        "blocks_per_year": "5256000"
    }
}
```

## Response Fields

| Field  | Type   | Description     |
| ------ | ------ | --------------- |
| params | object | Mint parameters |

## Use Cases

* **Tokenomics**: Read minting configuration

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
