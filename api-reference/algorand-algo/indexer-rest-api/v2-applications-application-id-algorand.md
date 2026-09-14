---
description: >-
  Example code for the /v2/applications/{application-id} REST method. Complete
  guide on how to use the /v2/applications/{application-id} REST method in the
  GetBlock Web3 documentation.
---

# /v2/applications/{application-id} - Algorand

Returns an application's parameters from the Indexer, including whether it has been deleted, the round the data applies to, and its global state.

## Endpoint

```http
GET /v2/applications/{application-id}
```

## Path Parameters

| Parameter      | Type    | Description    |
| -------------- | ------- | -------------- |
| application-id | integer | Application id |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_INDEXER=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_INDEXER}v2/applications/350338509"
```
{% endcode %}

## Response

```json
{
    "application": {
        "id": 350338509,
        "params": {
            "creator": "XBYLS2E6YI6XXL5BWCAMOA4GTWHXWENZMX5UHXMRNWWUQ7BXCY5WC5TEPA",
            "global-state": [
                {
                    "key": "Y291bnRlcg==",
                    "value": {
                        "type": 2,
                        "uint": 42
                    }
                }
            ]
        },
        "deleted": false
    },
    "current-round": 35000000
}
```

## Response Fields

| Field               | Type    | Description                             |
| ------------------- | ------- | --------------------------------------- |
| application.params  | object  | Application parameters and global state |
| application.deleted | boolean | Whether the application was deleted     |

## Use Cases

* **Contract Reads**: Read an application's global state
* **Explorers**: Render application pages
* **dApp Backends**: Inspect a contract's history

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 404 / Not found           | Not found     | No application matches the id                     |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
