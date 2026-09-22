# akash inflation params

Returns Akash's inflation module parameters, which govern token issuance and the community/provider incentive split.

## Endpoint

```http
GET /akash/inflation/v1beta3/params
```

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}akash/inflation/v1beta3/params"
```
{% endcode %}

## Response

```json
{
    "params": {
        "inflation_decay_factor": "0.75",
        "initial_inflation": "100.0",
        "variance": "0.05"
    }
}
```

## Response Fields

| Field  | Type   | Description                |
| ------ | ------ | -------------------------- |
| params | object | Akash inflation parameters |

## Use Cases

* **Tokenomics**: Read Akash-specific inflation config

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
