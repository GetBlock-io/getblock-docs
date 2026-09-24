---
description: >-
  Example code for the axelar/nexus/v1beta1/chains REST method. Complete guide
  on how to use axelar/nexus/v1beta1/chains REST method in GetBlock Web3
  documentation.
---

# /axelar/nexus/v1beta1/chains - Axelar

Returns all blockchains registered with the Axelar network — both Cosmos and EVM — that Axelar can route cross-chain messages and assets between.

## Endpoint

```http
GET /axelar/nexus/v1beta1/chains
```

## Example

{% code overflow="wrap" %}
```bash
export AXELAR_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AXELAR_REST}axelar/nexus/v1beta1/chains"
```
{% endcode %}

## Response

```json
{
    "chains": [
        "Ethereum",
        "Polygon",
        "avalanche",
        "osmosis",
        "Axelarnet",
        "binance"
    ]
}
```

## Response Fields

| Field  | Type  | Description                         |
| ------ | ----- | ----------------------------------- |
| chains | array | Names of chains connected to Axelar |

## Use Cases

* **Cross-Chain Routing**: List which chains Axelar connects
* **Integrations**: Validate a chain is supported before routing

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
