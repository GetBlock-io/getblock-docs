---
description: >-
  gRPC API reference for the Cronos Cosmos SDK modules. Explore service list,
  request examples, and how to connect to GetBlock's Cronos RPC endpoints
---

# Cosmos gRPC API - Cronos

The Cosmos SDK gRPC interface for Cronos: the same module query set as the REST API, exposed as typed Protocol Buffers services over HTTP/2. Examples use `grpcurl`.

## Methods

| Method                              | Description                         |
| ----------------------------------- | ----------------------------------- |
| auth Account                        | Account details and sequence        |
| bank Balance                        | Balance of one denom for an address |
| bank AllBalances                    | All balances for an address         |
| bank TotalSupply                    | Total supply of all denoms          |
| staking Validators                  | List staking validators             |
| staking DelegatorDelegations        | A delegator's delegations           |
| distribution DelegationTotalRewards | Outstanding rewards for a delegator |
| gov Proposals                       | List governance proposals           |
| tx BroadcastTx                      | Broadcast a signed transaction      |

## Support

* Support: [support@getblock.io](mailto:support@getblock.io)
