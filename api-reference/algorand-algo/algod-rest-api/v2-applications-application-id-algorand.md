---
description: >-
  Example code for the /v2/applications/{application-id} REST method. Complete
  guide on how to use the /v2/applications/{application-id} REST method in the
  GetBlock Web3 documentation.
---

# /v2/applications/{application-id} - Algorand

Returns the current parameters of an application (a stateful smart contract): its approval and clear programs, global state, and state schemas.

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
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}v2/applications/350338509"
```
{% endcode %}

## Response

```json
{
    "id": 350338509,
    "params": {
        "creator": "XBYLS2E6YI6XXL5BWCAMOA4GTWHXWENZMX5UHXMRNWWUQ7BXCY5WC5TEPA",
        "approval-program": "BYAB...",
        "clear-state-program": "BYAB...",
        "global-state": [
            {
                "key": "Y291bnRlcg==",
                "value": {
                    "type": 2,
                    "uint": 42
                }
            }
        ],
        "global-state-schema": {
            "num-uint": 1,
            "num-byte-slice": 0
        }
    }
}
```

## Response Fields

| Field                   | Type   | Description                          |
| ----------------------- | ------ | ------------------------------------ |
| params.creator          | string | Account that created the application |
| params.global-state     | array  | Global key/value state entries       |
| params.approval-program | string | Base64 TEAL approval program         |

## Use Cases

* **Contract Reads**: Read an application's global state
* **dApp Backends**: Inspect a smart contract
* **Explorers**: Render application pages

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 404 / Not found           | Not found     | No application with that id                       |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
