---
description: SUI JSON-RPC deprecated methods
---

# Deprecated JSON-RPC Methods

This section lists deprecated JSON-RPC methods for Sui. For new builds, use the current methods in [Sui (SUI)](../).

### Method groups

#### Objects and balances

Use these methods to read owned objects, coins, balances, and dynamic fields:

* `suix_getOwnedObjects`
* `suix_getCoins`
* `suix_getAllCoins`
* `suix_getBalance`
* `suix_getAllBalances`
* `suix_getDynamicFields`
* `suix_getDynamicFieldObject`
* `sui_getObject`
* `sui_multiGetObjects`
* `sui_tryGetPastObject`
* `sui_tryMultiGetPastObjects`

#### Transactions and events

Use these methods to inspect transactions, simulate execution, and query events:

* `sui_getTransactionBlock`
* `sui_multiGetTransactionBlocks`
* `suix_queryTransactionBlocks`
* `sui_executeTransactionBlock`
* `sui_devInspectTransactionBlock`
* `sui_dryRunTransactionBlock`
* `sui_getEvents`
* `suix_queryEvents`
* `suix_subscribeTransaction`
* `suix_subscribeEvent`

#### Move, checkpoints, and network state

Use these methods to read Move metadata, checkpoints, protocol state, and validator data:

* `sui_getNormalizedMoveFunction`
* `sui_getNormalizedMoveModule`
* `sui_getNormalizedMoveModulesByPackage`
* `sui_getNormalizedMoveStruct`
* `sui_getMoveFunctionArgTypes`
* `sui_getCheckpoint`
* `sui_getCheckpoints`
* `sui_getLatestCheckpointSequenceNumber`
* `sui_getProtocolConfig`
* `sui_getChainIdentifier`
* `sui_getTotalTransactionBlocks`
* `suix_getCommitteeInfo`
* `suix_getReferenceGasPrice`
* `suix_getLatestSuiSystemState`
* `suix_getValidatorsApy`
* `suix_getStakes`
* `suix_getStakesByIds`
* `suix_getTotalSupply`
* `suix_resolveNameServiceAddress`
* `suix_resolveNameServiceNames`

###
