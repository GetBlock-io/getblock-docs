# akash certificates

Returns mTLS certificates registered on-chain. Deployments and providers use these certificates to authenticate the client-provider connection.

## Endpoint

```
GET /akash/cert/v1beta3/certificates/list
```

## Query Parameters

| Parameter     | Type   | Description        |
| ------------- | ------ | ------------------ |
| filter.owner  | string | Owner address      |
| filter.state  | string | valid or revoked   |
| filter.serial | string | Certificate serial |

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}akash/cert/v1beta3/certificates/list?filter.owner=akash1nl7dg3xj9j2y6q7z0v4w8c2b5d1f3g6h9k2m4p"
```
{% endcode %}

## Response

```json
{
    "certificates": [
        {
            "certificate": {
                "state": "valid",
                "cert": "LS0tLS1CRUdJTi...",
                "pubkey": "LS0tLS1CRUdJTi..."
            },
            "serial": "12345"
        }
    ],
    "pagination": {
        "total": "1"
    }
}
```

## Response Fields

| Field        | Type  | Description                                   |
| ------------ | ----- | --------------------------------------------- |
| certificates | array | Registered certificates with state and serial |

## Use Cases

* **Auth**: List a user's deployment certificates
* **Providers**: Validate client certificates

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
