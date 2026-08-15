# Substrate API Sidecar (AssetHub + Relaychain)

Substrate API Sidecar is a REST service that runs on top of a Substrate node and returns decoded JSON, allowing clients to read chain data without computing storage keys or decoding SCALE. It complements the JSON-RPC surfaces with high-level, ready-to-use endpoints for accounts, blocks, staking, pallets, and transactions.

## Networks

On GetBlock's unified endpoint, **Asset Hub is the default network** and the **Relaychain is reached with an `/rc` path prefix**:

{% tabs %}
{% tab title="Asset Hub (default)" %}
```bash
curl 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/blocks/head'
```
{% endtab %}

{% tab title="Relaychain (/rc)" %}
```bash
curl 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/rc/blocks/head'
```
{% endtab %}
{% endtabs %}

Account, block, staking, and pallet endpoints work on both networks. Asset endpoints (assets, foreign assets, NFTs) are specific to Asset Hub; parachain (`/paras`) endpoints are specific to the Relaychain.

## Available Endpoints

### Accounts

| Endpoint                                   | Description                                                                                                                                          |
| ------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| `GET /accounts/{accountId}/balance-info`   | Returns the balance details for an account: free, reserved, and frozen amounts, the nonce, and any balance locks, at the latest or a specified block |
| `GET /accounts/{accountId}/staking-info`   | Returns the staking information for a stash account, including the controller, reward destination, and the bonded, active, and unlocking amounts     |
| `GET /accounts/{accountId}/asset-balances` | Returns an account's balances for asset hub assets                                                                                                   |
| `GET /accounts/{accountId}/vesting-info`   | Returns the vesting schedules for an account, including the locked amount, the per-block unlock, and the starting block of each schedule             |
| `GET /accounts/{accountId}/convert`        | Converts an account address between ss58 formats and its raw accountid, using the given prefix and scheme                                            |

### Blocks

| Endpoint                | Description                                                                                  |
| ----------------------- | -------------------------------------------------------------------------------------------- |
| `GET /blocks/head`      | Returns the most recent block, including its extrinsics, events grouped by phase, and author |
| `GET /blocks/{blockId}` | Returns a block by its number or hash, with decoded extrinsics and events                    |
| `GET /blocks`           | Returns a range of blocks in a single call, decoded like /blocks/{blockid}                   |

### Node

| Endpoint                     | Description                                                                              |
| ---------------------------- | ---------------------------------------------------------------------------------------- |
| `GET /node/version`          | Returns the node's client version, implementation name, and the chain it is connected to |
| `GET /node/transaction-pool` | Returns the extrinsics currently in the node's transaction pool                          |

### Pallets

| Endpoint                                          | Description                                                                                                                                    |
| ------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| `GET /pallets/{palletId}/storage/{storageItemId}` | Returns the decoded value of a storage item in a pallet, resolving the required keys                                                           |
| `GET /pallets/{palletId}/consts`                  | Returns the constants defined by a pallet, such as bonding durations or deposit amounts                                                        |
| `GET /pallets/staking/progress`                   | Returns the current staking system status: the active era, session progress, and the timing of the next era and unapplied slashes              |
| `GET /pallets/assets/{assetId}/asset-info`        | Returns the details and metadata of an asset hub asset, including its owner, total supply, minimum balance, and its name, symbol, and decimals |
| `GET /pallets/nomination-pools/{poolId}`          | Returns the details of a nomination pool: its state, points, member count, and bonded account                                                  |

### Runtime

| Endpoint                | Description                                                                                                                               |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| `GET /runtime/spec`     | Returns the runtime specification: the spec name and version, transaction version, and chain properties such as token symbol and decimals |
| `GET /runtime/metadata` | Returns the runtime metadata as decoded json                                                                                              |

### Transaction

| Endpoint                         | Description                                                                                                                                    |
| -------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| `GET /transaction/material`      | Returns the data needed to construct and sign a transaction offline: the genesis hash, chain name, spec and transaction versions, and metadata |
| `POST /transaction`              | Submits a signed, scale-encoded transaction to the network and returns its hash                                                                |
| `POST /transaction/fee-estimate` | Estimates the fee and weight for a submitted transaction without broadcasting it                                                               |

### Parachains

| Endpoint                    | Description                                                                               |
| --------------------------- | ----------------------------------------------------------------------------------------- |
| `GET /paras`                | Returns the parachains registered on the relay chain and their lifecycle status           |
| `GET /paras/leases/current` | Returns the current parachain lease period and the parachains that currently hold a lease |

## Support

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Substrate API Sidecar GitHub Repo](https://github.com/paritytech/substrate-api-sidecar)
* [Polkadot (DOT)](../)
