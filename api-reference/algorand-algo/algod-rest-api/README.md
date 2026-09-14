---
description: >-
  GetBlock provides fast and reliable access to Algorand nodes via the Algod
  REST API. Connect to the Algorand network without running your own
  infrastructure.
---

# Algod REST API

The complete algod REST interface for Algorand, generated from the official OpenAPI specification. Endpoints marked _(dedicated)_ are node-administration or diagnostic operations not served on GetBlock shared endpoints.

## Endpoints

* [GetConfig](/broken/pages/a49500e9ae5cc6ceca54425f9bf4e5fafb508827) _(dedicated)_ — `GET /debug/settings/config` — Gets the merged config file.
* [GetDebugSettingsProf](/broken/pages/89bc8214d90f5490f27d7c4c4a6813e07f295db1) _(dedicated)_ — `GET /debug/settings/pprof` — GetDebugSettingsProf
* [PutDebugSettingsProf](/broken/pages/5893d13335a06e0548bb6ebfc802c61602d7c7f5) _(dedicated)_ — `PUT /debug/settings/pprof` — PutDebugSettingsProf
* [GetGenesis](/broken/pages/df3bd3fb73bdbc94d4449f983fbe50cb43b8e250) — `GET /genesis` — Gets the genesis information.
* [HealthCheck](/broken/pages/de5d5dd07e52d95c640076e02de176c8b47464a7) — `GET /health` — Returns OK if healthy.
* [Metrics](/broken/pages/b0f1af4346cb4d72a7c789b97e28c1de8d93bc08) _(dedicated)_ — `GET /metrics` — Return metrics about algod functioning.
* [GetReady](/broken/pages/ea441a2875d1c467192bfa23cde2630bafd9c019) — `GET /ready` — Returns OK if healthy and fully caught up.
* [SwaggerJSON](/broken/pages/e2b344fbc537f9318bb7b57bfc69c3975178c9dc) _(dedicated)_ — `GET /swagger.json` — Gets the current swagger spec.
* [AccountInformation](/broken/pages/2027cb57b7e623ebc7e2867ac8952fbb8ceb0548) — `GET /v2/accounts/{address}` — Get account information.
* [AccountApplicationsInformation](/broken/pages/f3cc40075f1a8abf96a0b30cc4c44776502ac76b) — `GET /v2/accounts/{address}/applications` — Get a list of applications held by an account.
* [AccountApplicationInformation](/broken/pages/2f001806f5b422e50eed01ae1b1a423f3eb74f52) — `GET /v2/accounts/{address}/applications/{application-id}` — Get account information about a given app.
* [AccountAssetsInformation](/broken/pages/ff7a2efec9fe97e179b44e20c745a005015459f0) — `GET /v2/accounts/{address}/assets` — Get a list of assets held by an account, inclusive of asset params.
* [AccountAssetInformation](/broken/pages/de0767ca0484275f0a2417b09d46ff1cac60b897) — `GET /v2/accounts/{address}/assets/{asset-id}` — Get account information about a given asset.
* [GetPendingTransactionsByAddress](/broken/pages/7142e21445681fc0afd9867660a575758f4a1ee2) — `GET /v2/accounts/{address}/transactions/pending` — Get a list of unconfirmed transactions currently in the transaction pool by address.
* [GetApplicationByID](/broken/pages/3f45d0d5e3a41e09d014483aeff8c40d519d8948) — `GET /v2/applications/{application-id}` — Get application information.
* [GetApplicationBoxByName](/broken/pages/a5bb3935b697e218ea98384137bb8eda5103b007) — `GET /v2/applications/{application-id}/box` — Get box information for a given application.
* [GetApplicationBoxes](/broken/pages/3e102804d078fe73f5c53d8c87915fe0fb6191c6) — `GET /v2/applications/{application-id}/boxes` — Get all box names for a given application.
* [GetAssetByID](/broken/pages/0241b8bcbcf7b5612859448b065c1208b762c508) — `GET /v2/assets/{asset-id}` — Get asset information.
* [GetBlock](/broken/pages/2f94d2e38273e1be32fcbbdeea7517c6656d7fa3) — `GET /v2/blocks/{round}` — Get the block for the given round.
* [GetBlockHash](/broken/pages/f469e629f20e80bc8c5ac488dec63ff9b67cee66) — `GET /v2/blocks/{round}/hash` — Get the block hash for the block on the given round.
* [GetLightBlockHeaderProof](/broken/pages/31bcf67185118cf58df6e1cec39710552c79b495) — `GET /v2/blocks/{round}/lightheader/proof` — Gets a proof for a given light block header inside a state proof commitment
* [GetBlockLogs](/broken/pages/193a75fb9e3d55573c60e8592edd752abac953fc) — `GET /v2/blocks/{round}/logs` — Get all of the logs from outer and inner app calls in the given round
* [GetTransactionProof](/broken/pages/b25d21e108a4f259481918480018ff327deb7412) — `GET /v2/blocks/{round}/transactions/{txid}/proof` — Get a proof for a transaction in a block.
* [GetBlockTxids](/broken/pages/4f715d78f45185739dbb53035985e3fc40f0c58b) — `GET /v2/blocks/{round}/txids` — Get the top level transaction IDs for the block on the given round.
* [AbortCatchup](/broken/pages/989899fa77ac34eb34d558e1551382f76ea4c349) _(dedicated)_ — `DELETE /v2/catchup/{catchpoint}` — Aborts a catchpoint catchup.
* [StartCatchup](/broken/pages/5bd38ddd9a601e06a260c668a938cf9acbb9d5c2) _(dedicated)_ — `POST /v2/catchup/{catchpoint}` — Starts a catchpoint catchup.
* [GetLedgerStateDeltaForTransactionGroup](/broken/pages/f429a763f7b68925815a3d9ac8700dbe2ff4f7da) — `GET /v2/deltas/txn/group/{id}` — Get a LedgerStateDelta object for a given transaction group
* [GetLedgerStateDelta](/broken/pages/f911c4496d527bdd380add8254c9c7537007d946) — `GET /v2/deltas/{round}` — Get a LedgerStateDelta object for a given round
* [GetTransactionGroupLedgerStateDeltasForRound](/broken/pages/f634c5e3c1eda3396b86761f685f19f66efa9dc9) — `GET /v2/deltas/{round}/txn/group` — Get LedgerStateDelta objects for all transaction groups in a given round
* [GetBlockTimeStampOffset](/broken/pages/4bfe8c64b1dcfd466d37655ec16639788a9e504c) _(dedicated)_ — `GET /v2/devmode/blocks/offset` — Returns the timestamp offset. Timestamp offsets can only be set in dev mode.
* [SetBlockTimeStampOffset](/broken/pages/c5136009b6336158bb9e81fab16598b222493a50) _(dedicated)_ — `POST /v2/devmode/blocks/offset/{offset}` — Given a timestamp offset in seconds, adds the offset to every subsequent block header's timestamp.
* [ExperimentalCheck](/broken/pages/b4ad99f773185b6a13b92409d15bf8c0ed2092d6) — `GET /v2/experimental` — Returns OK if experimental API is enabled.
* [GetSupply](/broken/pages/ec4046018c5b1278aa25edc754faae5e02b56c6e) — `GET /v2/ledger/supply` — Get the current supply reported by the ledger.
* [UnsetSyncRound](/broken/pages/00a405877526e0358e51a2f9dbf9074251821743) _(dedicated)_ — `DELETE /v2/ledger/sync` — Removes minimum sync round restriction from the ledger.
* [GetSyncRound](/broken/pages/ff146411dd000f5d2131dccc14beacbb54fd2c8a) _(dedicated)_ — `GET /v2/ledger/sync` — Returns the minimum sync round the ledger is keeping in cache.
* [SetSyncRound](/broken/pages/e6d8b39ed182b21a9677b5cc9101d9e918517ce8) _(dedicated)_ — `POST /v2/ledger/sync/{round}` — Given a round, tells the ledger to keep that round in its cache.
* [GetPeers](/broken/pages/ca5f8543742723199a303109c57bff23eba7fa0a) — `GET /v2/node/peers` — GetPeers
* [ShutdownNode2](/broken/pages/cc4537d03fd75b02f4c8179823da77539855729b) — `POST /v2/node/shutdown` — ShutdownNode2
* [GetParticipationKeys](/broken/pages/d764daad7af0ca694bc42fde2e593affd65e382f) _(dedicated)_ — `GET /v2/participation` — Return a list of participation keys
* [AddParticipationKey](/broken/pages/e48ff48f995f42733573be0bad897f4b97734adf) _(dedicated)_ — `POST /v2/participation` — Add a participation key to the node
* [GenerateParticipationKeys](/broken/pages/9b09445c326f0221487d9472159d6ba8b5d8a4d5) _(dedicated)_ — `POST /v2/participation/generate/{address}` — Generate and install participation keys to the node.
* [DeleteParticipationKeyByID](/broken/pages/97d4354cd7feaa928452bbdcc148b0191d82438b) _(dedicated)_ — `DELETE /v2/participation/{participation-id}` — Delete a given participation key by ID
* [GetParticipationKeyByID](/broken/pages/0a0d47c448b403a576ad295c6cbfcfffa71c4614) _(dedicated)_ — `GET /v2/participation/{participation-id}` — Get participation key info given a participation ID
* [AppendKeys](/broken/pages/f77586b7c18b97e1b6687e80b84a84c1a0789471) _(dedicated)_ — `POST /v2/participation/{participation-id}` — Append state proof keys to a participation key
* [ShutdownNode](/broken/pages/72bc8401b21fe0f0d170d9e052d204824d0f2b34) _(dedicated)_ — `POST /v2/shutdown` — ShutdownNode
* [GetStateProof](/broken/pages/a1f4ab445315c19c48a9746b078b94d25936566e) — `GET /v2/stateproofs/{round}` — Get a state proof that covers a given round
* [GetStatus](/broken/pages/b14fd2dc09da160be200dc9584239a1874d77799) — `GET /v2/status` — Gets the current node status.
* [WaitForBlock](/broken/pages/0fa0f31442e7022072772b7d0868e5576645437f) — `GET /v2/status/wait-for-block-after/{round}` — Gets the node status after waiting for a round after the given round.
* [TealCompile](/broken/pages/5c0ba5dbd40e94ac9529e682122b707330390ca4) — `POST /v2/teal/compile` — Compile TEAL source code to binary, produce its hash
* [TealDisassemble](/broken/pages/4b66295d57865ee6f57331c8acd0bd0b785aa43f) — `POST /v2/teal/disassemble` — Disassemble program bytes into the TEAL source code.
* [RawTransaction](/broken/pages/50349fe0065186a687aa92c909e5c269e81251d3) — `POST /v2/transactions` — Broadcasts a raw transaction or transaction group to the network.
* [RawTransactionAsync](/broken/pages/ef42e7c98c3d4f53c6d41f823319e7f17e25f972) — `POST /v2/transactions/async` — Fast track for broadcasting a raw transaction or transaction group to the network through the tx handler without performing most of the checks and reporting detailed errors. Should be only used for development and performance testing.
* [TransactionParams](/broken/pages/dfcc2f8b0eb0aefc5bccc6a3a1b2242047634065) — `GET /v2/transactions/params` — Get parameters for constructing a new transaction
* [GetPendingTransactions](/broken/pages/8a5e4d85df3857a70c7c7f5bb2b0ab14a7464822) — `GET /v2/transactions/pending` — Get a list of unconfirmed transactions currently in the transaction pool.
* [PendingTransactionInformation](/broken/pages/fd544be45e66fdd6abfd3be56f3f3205897dc52e) — `GET /v2/transactions/pending/{txid}` — Get a specific pending transaction.
* [SimulateTransaction](/broken/pages/7a8d622daa166a2761bac8e51abeacf1cb142600) — `POST /v2/transactions/simulate` — Simulates a raw transaction or transaction group as it would be evaluated on the network. The simulation will use blockchain state from the latest committed round.
* [GetVersion](/broken/pages/3dd9a87f73250b562d8f6c7c3344ab243f56c969) — `GET /versions` — GetVersion

## OpenAPI

The machine-readable specification is included as `algod-openapi.yaml`.

## Support

* Support: [support@getblock.io](mailto:support@getblock.io)
