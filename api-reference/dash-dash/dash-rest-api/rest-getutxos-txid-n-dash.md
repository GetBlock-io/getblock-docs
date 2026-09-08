---
description: >-
  Example code for the /rest/getutxos/{txid}-{n} REST method. Complete guide on
  how to use the /rest/getutxos/{txid}-{n} REST method in the GetBlock Web3
  documentation.
---

# /rest/getutxos/{txid}-{n} - Dash

Returns whether the specified outpoints (txid-index pairs) are unspent, along with their values and scripts. Query the confirmed UTXO set by outpoint.

## Endpoint

```
GET /rest/getutxos/{txid}-{n}.json
```

## Path Parameters

| Parameter | Type   | Description                                 |
| --------- | ------ | ------------------------------------------- |
| txid-n    | string | One or more outpoints as -, slash-separated |

## Example

{% code overflow="wrap" %}
```bash
export DASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${DASH_REST}rest/getutxos/3a1f9c2e7b4d8a05f6c1e3d9b2a4c6e8f0d1b3a5c7e9f2d4b6a8c0e1f3d5b7a9c-0.json"
```
{% endcode %}

## Response

```json
{
    "chainHeight": 2100000,
    "bitmap": "1",
    "utxos": [
        {
            "height": 2099000,
            "value": 1.5,
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

| Field  | Type   | Description                           |
| ------ | ------ | ------------------------------------- |
| bitmap | string | Per-outpoint spent/unspent bitmap     |
| utxos  | array  | Unspent outputs with value and script |

## Use Cases

* **UTXO Checks**: Confirm an output is unspent before spending
* **Wallet Backends**: Validate inputs when building a tx
* **SPV**: Verify outputs for light clients

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 404 / Not Found           | Not found     | The outpoint format is invalid                    |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
