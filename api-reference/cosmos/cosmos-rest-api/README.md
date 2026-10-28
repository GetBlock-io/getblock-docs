---
description: >-
  GetBlock provides fast and reliable access to Cosmos nodes through the Cosmos
  SDK REST API.
---

# Cosmos REST API

The Cosmos SDK REST (LCD / gRPC-gateway) interface for Cosmos Hub: HTTP/JSON module queries for accounts, balances, staking, distribution, governance, and mint, plus the latest block, node info, and transaction broadcast. Addresses use the bech32 `cosmos1…` form; amounts are in the base denomination (uatom).

## Endpoint

{% code overflow="wrap" %}
```bash
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```
{% endcode %}

## Available Endpoints

### Accounts

| Endpoint              | Method | Description                                                                                                             |
| --------------------- | ------ | ----------------------------------------------------------------------------------------------------------------------- |
| auth-account          | GET    | Returns an account's details, including its account number and sequence (nonce), as a protobuf-Any wrapped account type |
| bank-balances         | GET    | Returns all token balances held by an address, paginated                                                                |
| bank-balance-by-denom | GET    | Returns the balance of a single denomination for an address                                                             |

### Network

| Endpoint       | Method | Description                                                                                                 |
| -------------- | ------ | ----------------------------------------------------------------------------------------------------------- |
| bank-supply    | GET    | Returns the total supply of all denominations on the chain, paginated                                       |
| mint-inflation | GET    | Returns the current minting inflation rate                                                                  |
| latest-block   | GET    | Returns the latest block via the Cosmos base tendermint service, including its header and transactions      |
| node-info      | GET    | Returns the node's information and the application's version details via the Cosmos base tendermint service |

### Staking

| Endpoint            | Method | Description                                                              |
| ------------------- | ------ | ------------------------------------------------------------------------ |
| staking-validators  | GET    | Returns all staking validators, optionally filtered by status, paginated |
| staking-validator   | GET    | Returns a single validator by its operator address                       |
| staking-delegations | GET    | Returns all delegations made by a delegator, paginated                   |
| staking-pool        | GET    | Returns the staking pool: the total bonded and not-bonded token amounts  |

### Distribution

| Endpoint             | Method | Description                                                                         |
| -------------------- | ------ | ----------------------------------------------------------------------------------- |
| distribution-rewards | GET    | Returns the outstanding staking rewards for a delegator, per validator and in total |

### Governance

| Endpoint      | Method | Description                                                            |
| ------------- | ------ | ---------------------------------------------------------------------- |
| gov-proposals | GET    | Returns governance proposals, optionally filtered by status, paginated |
| gov-proposal  | GET    | Returns a single governance proposal by its numeric identifier         |

### Transactions

| Endpoint     | Method | Description                                       |
| ------------ | ------ | ------------------------------------------------- |
| broadcast-tx | POST   | Broadcasts a signed, protobuf-encoded transaction |

## See Also

* [Cosmos overview](../)
* [Official Cosmos REST API Reference](https://docs.cosmos.network/api)
* [Cosmos SDK Module Specifications](https://docs.cosmos.network/main/build/modules)
