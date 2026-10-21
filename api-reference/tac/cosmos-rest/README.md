---
description: >-
  GetBlock provides fast and reliable access to TAC nodes via the Cosmos REST
  API. Connect to the TAC network without running your own infrastructure.
---

# Cosmos REST

The Cosmos SDK REST (LCD / gRPC-gateway) interface for TAC: HTTP/JSON module queries for accounts, balances, staking, distribution, and governance, plus the latest block and transaction broadcast. Addresses use the bech32 `tac1…` form; amounts are in the base denomination.

## Endpoint

{% code overflow="wrap" %}
```bash
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```
{% endcode %}

## Available Endpoints

### Accounts

| Endpoint      | Method | Description                                                                                                             |
| ------------- | ------ | ----------------------------------------------------------------------------------------------------------------------- |
| auth-account  | GET    | Returns an account's details, including its account number and sequence (nonce), as a protobuf-Any wrapped account type |
| bank-balances | GET    | Returns all token balances held by an address, paginated                                                                |

### Network

| Endpoint     | Method | Description                                                                                            |
| ------------ | ------ | ------------------------------------------------------------------------------------------------------ |
| bank-supply  | GET    | Returns the total supply of all denominations on the chain, paginated                                  |
| latest-block | GET    | Returns the latest block via the Cosmos base tendermint service, including its header and transactions |

### Staking

| Endpoint           | Method | Description                                                              |
| ------------------ | ------ | ------------------------------------------------------------------------ |
| staking-validators | GET    | Returns all staking validators, optionally filtered by status, paginated |
| staking-validator  | GET    | Returns a single validator by its operator address                       |
| staking-pool       | GET    | Returns the staking pool: the total bonded and not-bonded token amounts  |

### Distribution

| Endpoint             | Method | Description                                                                         |
| -------------------- | ------ | ----------------------------------------------------------------------------------- |
| distribution-rewards | GET    | Returns the outstanding staking rewards for a delegator, per validator and in total |

### Governance

| Endpoint      | Method | Description                                                            |
| ------------- | ------ | ---------------------------------------------------------------------- |
| gov-proposals | GET    | Returns governance proposals, optionally filtered by status, paginated |

### Transactions

| Endpoint     | Method | Description                                       |
| ------------ | ------ | ------------------------------------------------- |
| broadcast-tx | POST   | Broadcasts a signed, protobuf-encoded transaction |

## See Also

* [TAC overview](/broken/pages/3d68f19c295c4bf9aebed430b6e7dd2f2a109fb2)
* [Cosmos REST API Reference](https://docs.cosmos.network/api)
* [Cosmos SDK Module Specifications](https://docs.cosmos.network/main/build/modules)
