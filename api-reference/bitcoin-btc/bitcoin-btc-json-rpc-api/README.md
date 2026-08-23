# Bitcoin (BTC) JSON-RPC API

The Bitcoin (BTC) JSON-RPC API exposes the Bitcoin Core node interface: methods for reading blocks, transactions, and UTXOs, inspecting the mempool, building and broadcasting raw transactions, working with PSBTs, and estimating fees. Requests `POST` a JSON-RPC 2.0 body to the endpoint; the method is selected by the body.

### Base URL

```bash
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```

### Available API Methods

GetBlock provides access to Bitcoin Core JSON-RPC methods. Methods that require wallet or node-administration privileges are not available on shared endpoints and are omitted below.

#### Blockchain Methods

<table data-search="false"><thead><tr><th>Method</th><th>Description</th></tr></thead><tbody><tr><td>getbestblockhash</td><td>Returns the hash of the best (tip) block in the most-work fully-validated chain</td></tr><tr><td>getblock</td><td>Returns information about a block given its hash</td></tr><tr><td>getblockchaininfo</td><td>Returns an object with state information about blockchain processing, including the chain, height, best block hash, difficulty, verification progress, and softfork status</td></tr><tr><td>getblockcount</td><td>Returns the height of the most-work fully-validated chain</td></tr><tr><td>getblockhash</td><td>Returns the hash of the block at the given height in the best-block chain</td></tr><tr><td>getblockheader</td><td>Returns the block header for a given block hash</td></tr><tr><td>getblockstats</td><td>Computes per-block statistics for a given block, identified by hash or height</td></tr><tr><td>getchaintips</td><td>Returns information about all known tips in the block tree, including the main chain and any orphaned branches</td></tr><tr><td>getchaintxstats</td><td>Computes statistics about the total number and rate of transactions in the chain over a window of blocks ending at a given block</td></tr><tr><td>getdifficulty</td><td>Returns the proof-of-work difficulty as a multiple of the minimum difficulty</td></tr><tr><td>getindexinfo</td><td>Returns the status of the optional block indexes, such as the transaction index and the coinstats index, including whether each is synced and its best block height</td></tr><tr><td>gettxout</td><td>Returns details about an unspent transaction output (utxo)</td></tr><tr><td>gettxoutproof</td><td>Returns a hex-encoded proof that one or more transactions were included in a block</td></tr><tr><td>preciousblock</td><td>Treats a block as if it were received before others with the same work, changing tip selection</td></tr></tbody></table>

#### Mempool Methods

| Method                | Description                                                                                                                                   |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| getmempoolinfo        | Returns details about the active state of the transaction memory pool, including the number of transactions, total size, and minimum fee rate |
| getrawmempool         | Returns the transaction ids in the memory pool                                                                                                |
| getmempoolentry       | Returns mempool data for a specific transaction that is currently in the memory pool                                                          |
| getmempoolancestors   | Returns the in-mempool ancestors of a transaction: the unconfirmed transactions it depends on                                                 |
| getmempooldescendants | Returns the in-mempool descendants of a transaction: the unconfirmed transactions that depend on it                                           |
| testmempoolaccept     | Checks whether one or more raw transactions would be accepted into the mempool, without broadcasting them                                     |

#### Raw Transactions Methods

| Method                | Description                                                                                                                           |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| getrawtransaction     | Returns the raw transaction data for a given transaction id                                                                           |
| sendrawtransaction    | Submits a raw, fully signed transaction (serialized as hex) to the network                                                            |
| createrawtransaction  | Creates an unsigned raw transaction from a set of inputs and outputs and returns it as hex                                            |
| decoderawtransaction  | Decodes a serialized, hex-encoded transaction into a json object                                                                      |
| decodescript          | Decodes a hex-encoded script and returns details about it, including its assembly, type, and the addresses it pays to                 |
| combinerawtransaction | Combines multiple partially signed transactions of the same transaction into one fully signed transaction, returning the combined hex |

#### PSBT Methods

<table data-search="false"><thead><tr><th>Method</th><th>Description</th></tr></thead><tbody><tr><td>createpsbt</td><td>Creates an empty partially signed bitcoin transaction (psbt) from a set of inputs and outputs, returning it as a base64 string</td></tr><tr><td>decodepsbt</td><td>Decodes a base64-encoded psbt into a detailed json object, including the unsigned transaction, per-input data, and per-output data</td></tr><tr><td>analyzepsbt</td><td>Analyzes a psbt and reports what is missing for each input, the next role in the signing workflow, and the estimated fee and virtual size</td></tr><tr><td>combinepsbt</td><td>Combines multiple psbts of the same transaction into one, merging the data each contains</td></tr><tr><td>finalizepsbt</td><td>Finalizes a psbt once all inputs are signed and, if complete, extracts the network-ready transaction</td></tr><tr><td>converttopsbt</td><td>Converts a raw transaction to a psbt</td></tr><tr><td>joinpsbts</td><td>Joins multiple distinct psbts into a single psbt by concatenating their inputs and outputs</td></tr><tr><td>utxoupdatepsbt</td><td>Updates a psbt with utxo information and, optionally, output descriptors, filling in the data needed for signing from the node's view of the chain</td></tr></tbody></table>

#### Mining Methods

| Method           | Description                                                                                                             |
| ---------------- | ----------------------------------------------------------------------------------------------------------------------- |
| getmininginfo    | Returns mining-related information, including the current block height, difficulty, network hash rate, and mempool size |
| getnetworkhashps | Estimates the network hashes per second based on the last n blocks                                                      |
| getblocktemplate | Returns data needed to construct a block to work on, following bip 22, bip 23, bip 9, and bip 145                       |

#### Utility Methods

| Method           | Description                                                                                                                    |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| estimatesmartfee | Estimates the approximate fee rate per kilobyte needed for a transaction to begin confirmation within a given number of blocks |
| validateaddress  | Returns information about a bitcoin address, including whether it is valid and its script type                                 |
| deriveaddresses  | Derives one or more addresses from an output descriptor                                                                        |
| verifymessage    | Verifies a signed message against a bitcoin address, returning whether the signature is valid for the address and message      |

#### Control Methods

| Method        | Description                                                                                                                        |
| ------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| getrpcinfo    | Returns runtime details about the rpc server, including the commands currently in progress and the path to the active logging file |
| getmemoryinfo | Returns information about memory usage, either as a human-readable summary or as raw malloc statistics depending on the mode       |
| uptime        | Returns the total number of seconds the node has been running                                                                      |
| help          | Lists all available rpc commands, or returns detailed help for a specific command when one is supplied                             |
