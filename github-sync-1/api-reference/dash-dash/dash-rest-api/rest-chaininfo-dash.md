---
description: >-
  Example code for the /rest/chaininfo REST method. Complete guide on how to use
  the /rest/chaininfo REST method in the GetBlock Web3 documentation.
---

# /rest/chaininfo - Dash

Returns the same data as the getblockchaininfo RPC — chain name, height, best block hash, difficulty, and sync progress — over the REST interface.

## Endpoint

```
GET /rest/chaininfo.json
```

## Example

{% code overflow="wrap" %}
```bash
export DASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${DASH_REST}rest/chaininfo.json"
```
{% endcode %}

## Response

```json
{
    "chain": "main",
    "blocks": 2100000,
    "bestblockhash": "000000000000000abc12def34567890fedcba9876543210abcdef1234567890ff",
    "difficulty": 123456789.01,
    "verificationprogress": 0.9999
}
```

## Response Fields

| Field         | Type   | Description      |
| ------------- | ------ | ---------------- |
| chain         | string | Network name     |
| blocks        | number | Current height   |
| bestblockhash | string | Current tip hash |

## Use Cases

* **Chain Tip**: Read height and tip over REST
* **Status Pages**: Poll chain state without JSON-RPC
* **Health Checks**: Confirm the node is synced

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 404 / Not Found           | Not found     | The REST path is incorrect                        |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
