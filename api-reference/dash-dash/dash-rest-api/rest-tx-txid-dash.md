---
description: >-
  Example code for the /rest/tx/{txid} REST method. Complete guide on how to use
  the /rest/tx/{txid} REST method in the GetBlock Web3 documentation.
---

# /rest/tx/{txid} - Dash

Returns a decoded transaction as JSON for a given txid. Requires the node to maintain a transaction index for confirmed transactions.

## Endpoint

```http
GET /rest/tx/{txid}.json
```

## Path Parameters

| Parameter | Type   | Description    |
| --------- | ------ | -------------- |
| txid      | string | Transaction id |

## Example

{% code overflow="wrap" %}
```bash
export DASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${DASH_REST}rest/tx/3a1f9c2e7b4d8a05f6c1e3d9b2a4c6e8f0d1b3a5c7e9f2d4b6a8c0e1f3d5b7a9c.json"
```
{% endcode %}

## Response

```json
{
    "txid": "3a1f9c2e7b4d8a05f6c1e3d9b2a4c6e8f0d1b3a5c7e9f2d4b6a8c0e1f3d5b7a9c",
    "version": 3,
    "locktime": 0,
    "vin": [
        {
            "txid": "0x...",
            "vout": 0
        }
    ],
    "vout": [
        {
            "value": 1.5,
            "n": 0,
            "scriptPubKey": {
                "addresses": [
                    "XvKqL8m3nP7rT2wZ5aB9cD4eF6gH1jK0mN"
                ]
            }
        }
    ]
}
```

## Response Fields

| Field | Type  | Description         |
| ----- | ----- | ------------------- |
| vin   | array | Transaction inputs  |
| vout  | array | Transaction outputs |

## Use Cases

* **Transaction Reads**: Fetch a decoded tx over REST
* **Explorers**: Populate tx pages
* **Verification**: Confirm outputs

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 404 / Not Found           | Not found     | Unknown txid or txindex disabled                  |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
