---
description: >-
  GetBlock provides fast and reliable access to Cardano nodes via JSON-RPC API.
  Connect to the Cardano network without running your own infrastructure.
---

# Ogmios JSON-RPC API

Ogmios is a lightweight bridge to a Cardano node that speaks JSON-RPC 2.0. Over HTTP, it answers stateless request-response calls: ledger and network state queries, transaction submission, and script evaluation. Stateful protocols such as chain synchronization and mempool monitoring run over the WebSocket interface instead.

Every request is an HTTP `POST` with a JSON-RPC 2.0 body: a `method`, optional `params`, and an `id`. Responses carry the same `id` and either a `result` or an `error`.

## Base URL

```bash
https://go.getblock.io/<ACCESS-TOKEN>/
```

{% hint style="warning" %}
Replace `<ACCESS-TOKEN>` with the access token from the GetBlock dashboard.
{% endhint %}

## Network Queries

| Method                            | Description                      |
| --------------------------------- | -------------------------------- |
| queryNetwork/blockHeight          | Current network block height     |
| queryNetwork/tip                  | Network tip slot and hash        |
| queryNetwork/startTime            | Network start time               |
| queryNetwork/genesisConfiguration | Genesis configuration for an era |

## Ledger State Queries

| Method                                  | Description                             |
| --------------------------------------- | --------------------------------------- |
| queryLedgerState/tip                    | Ledger tip slot and hash                |
| queryLedgerState/epoch                  | Current epoch                           |
| queryLedgerState/eraStart               | Start of the current era                |
| queryLedgerState/eraSummaries           | Era summaries for time conversion       |
| queryLedgerState/protocolParameters     | Current protocol parameters             |
| queryLedgerState/utxo                   | Unspent outputs by address or reference |
| queryLedgerState/stakePools             | Registered stake pools                  |
| queryLedgerState/liveStakeDistribution  | Live stake distribution                 |
| queryLedgerState/rewardAccountSummaries | Reward-account summaries                |
| queryLedgerState/projectedRewards       | Projected rewards                       |
| queryLedgerState/rewardsProvenance      | Rewards provenance data                 |
| queryLedgerState/nonces                 | Consensus nonces                        |
| queryLedgerState/treasuryAndReserves    | Treasury and reserves balances          |
| queryLedgerState/governanceProposals    | Active governance proposals             |

## Transaction Submission

| Method              | Description                     |
| ------------------- | ------------------------------- |
| submitTransaction   | Submit a signed transaction     |
| evaluateTransaction | Evaluate script execution units |

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Ogmios Documentation](https://ogmios.dev/)
* [Ogmios WebSocket API](/broken/pages/81d53e41f3a5dbfe7f3a29648e2f16079e58bd29)
* [Cardano (ADA)](/broken/pages/88e53d36b23986384e9b0340ba88b162a80e78d7)
