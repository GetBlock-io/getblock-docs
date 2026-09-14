---
description: >-
  Example code for the /genesis REST method. Complete guide on how to use the
  /genesis REST method in the GetBlock Web3 documentation.
---

# /genesis - Algorand

Returns the genesis configuration the node was started with, including the genesis id, hash, and initial allocations. Fixed for the life of the network.

## Endpoint

```http
GET /genesis
```

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}genesis"
```
{% endcode %}

## Response

```json
{
    "id": "mainnet-v1.0",
    "network": "mainnet",
    "proto": "https://github.com/algorandfoundation/specs/tree/...",
    "fees": "A7NMWS3NT3IUDMLVO26ULGXGIIOUQ3ND2TXSER6EBGRZNObOMKA",
    "rwd": "737..."
}
```

## Response Fields

| Field   | Type   | Description                    |
| ------- | ------ | ------------------------------ |
| id      | string | Genesis id (e.g. mainnet-v1.0) |
| network | string | Network name                   |
| proto   | string | Initial consensus protocol     |

## Use Cases

* **Network Identification**: Confirm which network the node serves
* **Genesis Hash**: Read the genesis hash for transaction signing
* **Tooling Setup**: Seed clients with genesis constants

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| 500 / Internal            | Internal error | The node failed to return genesis                 |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
