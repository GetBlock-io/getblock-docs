---
description: >-
  Example code for the akash/cert/v1/certificates/list REST method. Complete
  guide on how to use akash/cert/v1/certificates/list REST method in GetBlock
  Web3 documentation.
---

# /akash/cert/v1/certificates/list - Akash

Returns mTLS certificates registered on-chain. Deployments and providers use these certificates to authenticate the client-provider connection.

## Endpoint

```
GET /akash/cert/v1/certificates/list
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

curl "${AKASH_REST}akash/cert/v1/certificates/list?pagination.limit=1"
```
{% endcode %}

## Response

```json
{
    "certificates": [
        {
            "certificate": {
                "state": "valid",
                "cert": "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tDQpNSUlCbXpDQ0FVR2dBd0lCQWdJSEJpbURkQVhLR0RB...",
                "pubkey": "LS0tLS1CRUdJTiBFQyBQVUJMSUMgS0VZLS0tLS0NCk1Ga3dFd1lIS29aSXpqMENBUVlJS29aSXpqMERB..."
            },
            "serial": "1734494424255000"
        }
    ],
    "pagination": {
        "next_key": "rChFCQIBAgIRAR0UAABWKhORnoNQejBl+6jeybTC4AoHBi1xezFhIA==",
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
