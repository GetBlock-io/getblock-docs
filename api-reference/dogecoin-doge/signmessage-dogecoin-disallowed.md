---
description: >-
  Example code for the signmessage JSON-RPC method. Complete guide on how to use
  signmessage JSON-RPC in GetBlock Web3 documentation.
---

# signmessage - Dogecoin {disallowed}

This method signs a message with the private key of an address.

{% hint style="info" %}
* Requires the private key for the address to be in the wallet.
* May not work on shared nodes if wallet access is restricted.
* Use `verifymessage` to verify signatures.
{% endhint %}

## Parameters

| Parameter | Type   | Required | Description                        |
| --------- | ------ | -------- | ---------------------------------- |
| address   | string | Yes      | The Dogecoin address to sign with. |
| message   | string | Yes      | The message to sign.               |

## Request

{% include "../../.gitbook/includes/curl-location-request-p... (1).md" %}

## Response

{% code title="Response (JSON)" %}
```json
{
    "result": "H1b2c3d4e5f6g7h8i9j0...",
    "error": null,
    "id": "getblock.io"
}
```
{% endcode %}

## Response Parameters

| Field  | Type   | Description                      |
| ------ | ------ | -------------------------------- |
| result | string | The base64-encoded signature.    |
| error  | null   | Error object (null if no error). |
| id     | string | Request identifier.              |

## Use Case

The `signmessage` method is essential for:

* Proving address ownership
* Authentication systems
* Cryptographic verification
* Digital signatures
* Identity verification

## Error Handling

| Error Code | Message                   | Cause                            |
| ---------- | ------------------------- | -------------------------------- |
| -3         | Invalid address           | Invalid Dogecoin address.        |
| -4         | Private key not available | Address not in wallet.           |
| 403        | Forbidden                 | Missing or invalid ACCESS-TOKEN. |
