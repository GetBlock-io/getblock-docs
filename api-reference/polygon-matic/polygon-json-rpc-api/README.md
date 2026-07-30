---
description: >-
  GetBlock provides fast and reliable access to Polygon nodes via the JSON-RPC
  API. Connect to the Polygon network without running your own infrastructure.
---

# Polygon JSON-RPC API

The Polygon JSON-RPC API is the standard interface developers use to communicate with the Polygon blockchain. It allows decentralized applications (dApps) to read on-chain data, query smart contracts, and submit transactions without the need to run a local copy of a full blockchain node

### API Method Categories

#### Account Methods

Methods for retrieving account-related information:

* `eth_accounts` - Returns accounts managed by the node
* `eth_getBalance` - Returns account balance
* `eth_getTransactionCount` - Returns transaction count (nonce)
* `eth_getCode` - Returns contract bytecode
* `eth_getStorageAt` - Returns storage value at position
* `eth_getProof` - Returns Merkle proof for account state

#### Block Methods

Methods for retrieving block information:

* `eth_blockNumber` - Returns latest block number
* `eth_getBlockByHash` - Returns block by hash
* `eth_getBlockByNumber` - Returns block by number
* `eth_getBlockTransactionCountByHash` - Returns transaction count in block by hash
* `eth_getBlockTransactionCountByNumber` - Returns transaction count in block by number

#### Transaction Methods

Methods for transaction operations:

* `eth_getTransactionByHash` - Returns transaction by hash
* `eth_getTransactionByBlockHashAndIndex` - Returns transaction by block hash and index
* `eth_getTransactionByBlockNumberAndIndex` - Returns transaction by block number and index
* `eth_getTransactionReceipt` - Returns transaction receipt
* `eth_sendRawTransaction` - Submits signed transaction
* `eth_sendTransaction` - Creates and sends transaction
* `eth_sign` - Signs data with account
* `eth_pendingTransactions` - Returns pending transactions

#### Smart Contract Methods

Methods for smart contract interaction:

* `eth_call` - Executes contract call without transaction
* `eth_estimateGas` - Estimates gas for transaction

#### Filter Methods

Methods for event filtering and subscription:

* `eth_newFilter` - Creates log filter
* `eth_newBlockFilter` - Creates block filter
* `eth_newPendingTransactionFilter` - Creates pending transaction filter
* `eth_getFilterChanges` - Returns filter changes
* `eth_getFilterLogs` - Returns filter logs
* `eth_getLogs` - Returns logs matching criteria
* `eth_uninstallFilter` - Removes filter
* `eth_subscribe` - Creates subscription (WebSocket)
* `eth_unsubscribe` - Removes subscription (WebSocket)

#### Gas Methods

Methods for gas price information:

* `eth_gasPrice` - Returns current gas price
* `eth_feeHistory` - Returns historical gas fees
* `eth_maxPriorityFeePerGas` - Returns max priority fee suggestion

#### Network Methods

Methods for network information:

* `eth_chainId` - Returns chain ID
* `eth_syncing` - Returns sync status
* `eth_protocolVersion` - Returns protocol version
* `net_version` - Returns network ID
* `net_listening` - Returns listening status
* `net_peerCount` - Returns peer count

#### Node Methods

Methods for node information:

* `eth_coinbase` - Returns coinbase address
* `eth_mining` - Returns mining status
* `eth_hashrate` - Returns hashrate
* `rpc_modules` - Returns enabled RPC modules
* `web3_clientVersion` - Returns client version
* `web3_sha3` - Returns Keccak-256 hash

#### Uncle Methods

Methods for uncle block information:

* `eth_getUncleByBlockHashAndIndex` - Returns uncle by block hash and index
* `eth_getUncleByBlockNumberAndIndex` - Returns uncle by block number and index
* `eth_getUncleCountByBlockHash` - Returns uncle count by block hash
* `eth_getUncleCountByBlockNumber` - Returns uncle count by block number
