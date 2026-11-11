---
description: >-
  GetBlock provides fast and reliable access to Cosmos nodes through the gRPC
  API.
---

# Cosmos gRPC

The Cosmos SDK gRPC interface for Cosmos Hub: the full module query set exposed as typed Protocol Buffers services over HTTP/2. gRPC carries the same data as the [Cosmos REST API](cosmos-rest-api/) (a RESTful HTTP/JSON gateway generated in front of these gRPC services), plus transaction, block, and node services. Examples use `grpcurl`. Addresses use the bech32 `cosmos1…` form; amounts are in the base denomination (uatom, 1 ATOM = 1,000,000 uatom).

## Endpoint

```bash
shared.eu-central-1.getblock.io:443   (TLS, access token passed as request metadata)
```

{% hint style="info" %}
Replace `<ACCESS-TOKEN>` with the access token from the GetBlock dashboard. gRPC connects to a `host:port` target over TLS, with the access token supplied as request metadata rather than in the URL path. Confirm the exact gRPC target and the access-token metadata key in the GetBlock dashboard. The testnet (`provider`) is served from the same Frankfurt region.
{% endhint %}

## Discovering Services

If server reflection is enabled, list every service and its methods directly from the node — this is the authoritative, always-current surface for the deployed version:

```bash
# List all gRPC services
grpcurl -H "x-api-key: <ACCESS-TOKEN>" \
  shared.eu-central-1.getblock.io:443 list

# List the methods of one service
grpcurl -H "x-api-key: <ACCESS-TOKEN>" \
  shared.eu-central-1.getblock.io:443 list cosmos.bank.v1beta1.Query

# Describe a method's request/response types
grpcurl -H "x-api-key: <ACCESS-TOKEN>" \
  shared.eu-central-1.getblock.io:443 describe cosmos.bank.v1beta1.Query.AllBalances
```

## Available Services

The services below are the standard set for Cosmos SDK queries and services. Each is addressed as `<service>/<Method>` (for example `cosmos.bank.v1beta1.Query/AllBalances`).

### Auth — `cosmos.auth.v1beta1.Query`

<table data-search="false"><thead><tr><th>Method</th><th>Description</th></tr></thead><tbody><tr><td>Accounts</td><td>All accounts, paginated</td></tr><tr><td>Account</td><td>A single account by address</td></tr><tr><td>AccountAddressByID</td><td>Account address by account number</td></tr><tr><td>AccountInfo</td><td>Account info (base account fields) for an address</td></tr><tr><td>Params</td><td>Auth module parameters</td></tr><tr><td>ModuleAccounts</td><td>All module accounts</td></tr><tr><td>ModuleAccountByName</td><td>A module account by name</td></tr><tr><td>Bech32Prefix</td><td>The chain's bech32 account-address prefix (cosmos)</td></tr><tr><td>AddressBytesToString</td><td>Encode address bytes to a bech32 string</td></tr><tr><td>AddressStringToBytes</td><td>Decode a bech32 string to address bytes</td></tr></tbody></table>

### Authz — `cosmos.authz.v1beta1.Query`

| Method        | Description                                        |
| ------------- | -------------------------------------------------- |
| Grants        | Authorization grants between a granter and grantee |
| GranterGrants | All grants issued by a granter                     |
| GranteeGrants | All grants received by a grantee                   |

### Bank — `cosmos.bank.v1beta1.Query`

| Method                  | Description                                   |
| ----------------------- | --------------------------------------------- |
| Balance                 | Balance of one denom for an address           |
| AllBalances             | All balances for an address                   |
| SpendableBalances       | Spendable balances for an address             |
| SpendableBalanceByDenom | Spendable balance of one denom for an address |
| TotalSupply             | Total supply of all denoms                    |
| SupplyOf                | Total supply of one denom                     |
| Params                  | Bank module parameters                        |
| DenomMetadata           | Metadata for one denom                        |
| DenomsMetadata          | Metadata for all denoms                       |
| DenomOwners             | Addresses owning a given denom                |
| SendEnabled             | Send-enabled status per denom                 |

### Staking — `cosmos.staking.v1beta1.Query`

| Method                        | Description                                    |
| ----------------------------- | ---------------------------------------------- |
| Validators                    | All validators, optionally filtered by status  |
| Validator                     | A single validator by operator address         |
| ValidatorDelegations          | Delegations to a validator                     |
| ValidatorUnbondingDelegations | Unbonding delegations from a validator         |
| Delegation                    | A delegation between a delegator and validator |
| UnbondingDelegation           | An unbonding delegation                        |
| DelegatorDelegations          | A delegator's delegations                      |
| DelegatorUnbondingDelegations | A delegator's unbonding delegations            |
| Redelegations                 | Redelegations for a delegator                  |
| DelegatorValidators           | Validators a delegator is bonded to            |
| DelegatorValidator            | One validator a delegator is bonded to         |
| HistoricalInfo                | Historical header/validator info at a height   |
| Pool                          | Bonded and not-bonded token pools              |
| Params                        | Staking module parameters                      |

### Distribution — `cosmos.distribution.v1beta1.Query`

| Method                      | Description                               |
| --------------------------- | ----------------------------------------- |
| Params                      | Distribution module parameters            |
| ValidatorDistributionInfo   | A validator's distribution info           |
| ValidatorOutstandingRewards | A validator's outstanding rewards         |
| ValidatorCommission         | A validator's accumulated commission      |
| ValidatorSlashes            | A validator's slash events                |
| DelegationRewards           | Rewards of a delegation to one validator  |
| DelegationTotalRewards      | Total outstanding rewards for a delegator |
| DelegatorValidators         | Validators a delegator has rewards with   |
| DelegatorWithdrawAddress    | A delegator's withdraw address            |
| CommunityPool               | The community pool balance                |

### Governance — `cosmos.gov.v1.Query`

| Method       | Description                                    |
| ------------ | ---------------------------------------------- |
| Constitution | The chain constitution                         |
| Proposal     | A single proposal by ID                        |
| Proposals    | Proposals, optionally filtered by status       |
| Vote         | A vote on a proposal by a voter                |
| Votes        | All votes on a proposal                        |
| Params       | Governance parameters (voting, tally, deposit) |
| Deposit      | A deposit on a proposal by a depositor         |
| Deposits     | All deposits on a proposal                     |
| TallyResult  | The current tally for a proposal               |

### Mint — `cosmos.mint.v1beta1.Query`

| Method           | Description                    |
| ---------------- | ------------------------------ |
| Params           | Mint module parameters         |
| Inflation        | Current minting inflation rate |
| AnnualProvisions | Current annual provisions      |

### Slashing — `cosmos.slashing.v1beta1.Query`

| Method       | Description                     |
| ------------ | ------------------------------- |
| Params       | Slashing module parameters      |
| SigningInfo  | Signing info for one validator  |
| SigningInfos | Signing info for all validators |

### Feegrant — `cosmos.feegrant.v1beta1.Query`

| Method              | Description                                   |
| ------------------- | --------------------------------------------- |
| Allowance           | A fee allowance between a granter and grantee |
| Allowances          | All fee allowances for a grantee              |
| AllowancesByGranter | All fee allowances issued by a granter        |

### Evidence — `cosmos.evidence.v1beta1.Query`

| Method      | Description                      |
| ----------- | -------------------------------- |
| Evidence    | A single evidence record by hash |
| AllEvidence | All evidence records             |

### Params — `cosmos.params.v1beta1.Query`

| Method    | Description                             |
| --------- | --------------------------------------- |
| Params    | A raw parameter by subspace and key     |
| Subspaces | Registered parameter subspaces and keys |

### Upgrade — `cosmos.upgrade.v1beta1.Query`

| Method         | Description                               |
| -------------- | ----------------------------------------- |
| CurrentPlan    | The current upgrade plan, if any          |
| AppliedPlan    | The height an applied plan executed at    |
| ModuleVersions | Module consensus versions                 |
| Authority      | The module's governance authority address |

### IBC — `ibc.core.channel.v1.Query` / `ibc.applications.transfer.v1.Query`

| Method      | Description                                                       |
| ----------- | ----------------------------------------------------------------- |
| Channels    | All IBC channels (ibc.core.channel.v1)                            |
| Channel     | A single IBC channel by port and channel id                       |
| DenomTrace  | The denom trace for an IBC voucher (ibc.applications.transfer.v1) |
| DenomTraces | All IBC denom traces                                              |

### Transactions — `cosmos.tx.v1beta1.Service`

| Method          | Description                                          |
| --------------- | ---------------------------------------------------- |
| BroadcastTx     | Broadcast a signed transaction (SYNC/ASYNC)          |
| Simulate        | Simulate a transaction and return gas/result         |
| GetTx           | Fetch a transaction by hash                          |
| GetTxsEvent     | Search transactions by events, paginated             |
| GetBlockWithTxs | Fetch a block together with its decoded transactions |
| TxDecode        | Decode transaction bytes to a Tx                     |
| TxEncode        | Encode a Tx to transaction bytes                     |

### Node & Base — `cosmos.base.tendermint.v1beta1.Service` / `cosmos.base.node.v1beta1.Service`

| Method                  | Description                              |
| ----------------------- | ---------------------------------------- |
| GetNodeInfo             | Node and application (gaia) version info |
| GetSyncing              | Whether the node is syncing              |
| GetLatestBlock          | The latest block                         |
| GetBlockByHeight        | A block at a given height                |
| GetLatestValidatorSet   | The latest validator set                 |
| GetValidatorSetByHeight | The validator set at a given height      |
| ABCIQuery               | A low-level ABCI query (node service)    |
| Config                  | Node configuration (minimum gas price)   |
| Status                  | Node sync status                         |

## Example

List services, then query a bank balance:

```bash
# All balances for an address
grpcurl -H "x-api-key: <ACCESS-TOKEN>" \
  -d '{"address":"cosmos1xv9tklw7d82sezh9haa573wufgy59vmwe6xxe5"}' \
  shared.eu-central-1.getblock.io:443 \
  cosmos.bank.v1beta1.Query/AllBalances

# A validator by operator address
grpcurl -H "x-api-key: <ACCESS-TOKEN>" \
  -d '{"validator_addr":"cosmosvaloper1xv9tklw7d82sezh9haa573wufgy59vmwnp5s7g"}' \
  shared.eu-central-1.getblock.io:443 \
  cosmos.staking.v1beta1.Query/Validator
```

## Response

```json
{
  "balances": [
    { "denom": "uatom", "amount": "125000000" }
  ],
  "pagination": { "total": "1" }
}
```

{% hint style="info" %}
The metadata key for the access token (`x-api-key` above) is illustrative — confirm the exact header the GetBlock gRPC gateway expects in the dashboard.
{% endhint %}

## Use Cases

* **Typed backends**: Use generated protobuf clients for production services
* **Same query set as REST**: Prefer gRPC for performance and typing; REST for quick HTTP/JSON access
* **Transaction broadcast**: Submit signed transactions via `cosmos.tx.v1beta1.Service/BroadcastTx`
* **Live discovery**: Use `grpcurl list` / `describe` to read the exact surface of the deployed node

## See Also

* [Cosmos overview](./)
* [Cosmos REST API](cosmos-rest-api/) — the same query set over HTTP/JSON
* [Cosmos SDK gRPC documentation](https://docs.cosmos.network/sdk/latest/api-reference/grpc)
* [Cosmos SDK Module Specifications](https://docs.cosmos.network/main/build/modules)
