# base node info

Returns node and application version info via the Cosmos base service.

## Endpoint

```http
GET /cosmos/base/tendermint/v1beta1/node_info
```

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}cosmos/base/tendermint/v1beta1/node_info"
```
{% endcode %}

## Response

```json
{
    "default_node_info": {
        "network": "akashnet-2",
        "version": "0.38.0"
    },
    "application_version": {
        "name": "akash",
        "version": "0.38.0"
    }
}
```

## Response Fields

| Field                | Type   | Description              |
| -------------------- | ------ | ------------------------ |
| default\_node\_info  | object | Node network and version |
| application\_version | object | Application build info   |

## Use Cases

* **Diagnostics**: Report node/app versions

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
