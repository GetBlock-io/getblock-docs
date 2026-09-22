---
description: >-
  GetBlock provides fast and reliable access to Akash nodes via the Cosmos SDK
  REST API. Connect to the Akash network without running your own
  infrastructure.
---

# Cosmos REST API - Akash

The Cosmos SDK REST (LCD) interface for Akash: HTTP/JSON queries for accounts, balances, staking, distribution, and governance, plus Akash's own marketplace modules — deployments, orders, bids, leases, and providers. Addresses use the bech32 `akash1…` form.

## Methods

| Method                      | Description                        |
| --------------------------- | ---------------------------------- |
| auth-account                | Account details and sequence       |
| bank-balances               | All token balances for an address  |
| bank-supply                 | Total supply of all denoms         |
| staking-validators          | List staking validators            |
| staking-pool                | Bonded and not-bonded pools        |
| distribution-rewards        | Outstanding staking rewards        |
| gov-proposals               | List governance proposals          |
| latest-block                | Latest block (Cosmos base)         |
| broadcast-tx                | Broadcast a signed transaction     |
| akash-deployments           | List deployments                   |
| akash-deployment            | Get a deployment by ID             |
| akash-orders                | List market orders                 |
| akash-bids                  | List market bids                   |
| akash-leases                | List leases                        |
| akash-providers             | List providers                     |
| akash-provider              | Get a provider by owner            |
| auth-params                 | Auth module parameters             |
| bank-supply-denom           | Supply of a single denom           |
| staking-validator           | A single validator                 |
| staking-delegations         | A delegator's delegations          |
| mint-inflation              | Current inflation rate             |
| mint-params                 | Mint module parameters             |
| distribution-community-pool | Community pool balance             |
| gov-proposal                | A single proposal                  |
| gov-tally                   | Current vote tally                 |
| slashing-params             | Slashing parameters                |
| upgrade-current-plan        | Current upgrade plan               |
| base-node-info              | Node info (Cosmos base)            |
| base-validatorsets-latest   | Latest validator set (Cosmos base) |
| simulate                    | Simulate a transaction             |
| tx-by-hash                  | Transaction by hash (Cosmos)       |
| akash-deployment-groups     | Get a deployment group             |
| akash-order                 | Get a market order                 |
| akash-bid                   | Get a market bid                   |
| akash-lease                 | Get a lease                        |
| akash-certificates          | List certificates                  |
| akash-escrow-accounts       | List escrow accounts               |
| akash-escrow-payments       | List escrow payments               |
| akash-audit-providers       | List audited provider attributes   |
| akash-inflation-params      | Akash inflation parameters         |
| akash-take-params           | Marketplace take (fee) parameters  |

## Support

* Support: [support@getblock.io](mailto:support@getblock.io)
