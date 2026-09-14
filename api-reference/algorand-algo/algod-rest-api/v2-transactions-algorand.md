---
description: >-
  Example code for the /v2/transactions REST method. Complete guide on how to
  use the /v2/transactions REST method in the GetBlock Web3 documentation.
---

# /v2/transactions - Algorand

Broadcasts one or more signed, msgpack-encoded transactions to the network and returns the transaction id. The request body is the raw signed transaction bytes (`Content-Type: application/x-binary`).

## Endpoint

```http
POST /v2/transactions
```

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl --location --request POST "${ALGO_ALGOD}v2/transactions" \
--header 'Content-Type: application/x-binary' \
--data-binary '<raw msgpack-encoded signed transaction bytes>'
```
{% endcode %}

## Response

```json
{
    "txId": "MEFEHTKVA6LP4CIFC3VNJNXFXPBPHQMBNK5CGWQENTNJXCTS3ELA"
}
```

## Response Fields

| Field | Type   | Description                                 |
| ----- | ------ | ------------------------------------------- |
| txId  | string | Transaction id of the submitted transaction |

## Use Cases

* **Transaction Submission**: Broadcast a signed payment or app call
* **Wallet Backends**: Submit user-signed transactions
* **dApp Writes**: Send application calls to the network

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | The transaction failed validation or decoding     |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
