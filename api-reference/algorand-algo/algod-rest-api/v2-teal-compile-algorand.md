---
description: >-
  Example code for the /v2/teal/compile REST method. Complete guide on how to
  use the /v2/teal/compile REST method in the GetBlock Web3 documentation.
---

# /v2/teal/compile - Algorand

Compiles TEAL source code into the compiled program bytes and the resulting logic-signature address. The request body is the TEAL source (`Content-Type: text/plain`). Must be enabled on the node.

## Endpoint

```http
POST /v2/teal/compile
```

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl --location --request POST "${ALGO_ALGOD}v2/teal/compile" \
--header 'Content-Type: application/x-binary' \
--data-binary '#pragma version 8
int 1
return'
```
{% endcode %}

## Response

```json
{
    "hash": "6Z3C3LDVWGMX23BMSYMANACQOSINPFIRF7XVHDHQMZP2N54VDT2GJ6M4A5",
    "result": "CCABASI="
}
```

## Response Fields

| Field  | Type   | Description                                     |
| ------ | ------ | ----------------------------------------------- |
| result | string | Base64-compiled program bytes                   |
| hash   | string | Logic-signature address of the compiled program |

## Use Cases

* **Smart Contracts**: Compile TEAL for deployment or logic sigs
* **Tooling**: Server-side TEAL compilation
* **LogicSig Addresses**: Derive the address of a compiled program

## Error Handling

| Error                     | Message              | Description                                       |
| ------------------------- | -------------------- | ------------------------------------------------- |
| 404 / Disabled            | Compilation disabled | TEAL compilation is not enabled on the node       |
| 403 / RBAC: access denied | Access denied        | The GetBlock access token is missing or incorrect |
