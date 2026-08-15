# Polkadot JSON-RPC API (Relaychain)

The Polkadot JSON-RPC API exposes the relay chain's Substrate node interface over JSON-RPC 2.0. Methods are grouped into namespaces (`chain_`, `state_`, `system_`, `payment_`, `grandpa_`, and others). Requests `POST` a JSON-RPC 2.0 body to the endpoint; the method is selected by the body.

This reference documents the stable HTTP methods. GetBlock's node also exposes WebSocket subscription methods (`chain_subscribe*`, `state_subscribe*`, `grandpa_subscribe*`), the newer JSON-RPC spec (`chainHead_v1_*`, `chainSpec_v1_*`, `transaction_v1_*`), and node-administration methods; those are reached over WebSocket or restricted on shared endpoints.

## Base URL

{% tabs %}
{% tab title="HTTPS" %}
```bash
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}

{% tab title="WebSocket" %}
```bash
wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

The Polkadot.js API (`@polkadot/api`) connects over the WebSocket endpoint. Each method page below includes a Polkadot.js example.

## Available Methods

### Account

| Method                   | Description                                                                                  |
| ------------------------ | -------------------------------------------------------------------------------------------- |
| account\_nextIndex       | Returns the next nonce for an account, including any transactions already queued in the pool |
| system\_accountNextIndex | Returns the next nonce for an account, including transactions already in the pool            |

### Author

| Method                    | Description                                                                                                |
| ------------------------- | ---------------------------------------------------------------------------------------------------------- |
| author\_pendingExtrinsics | Returns the extrinsics currently in the node's transaction pool that are waiting to be included in a block |
| author\_submitExtrinsic   | Submits a signed extrinsic to the node's transaction pool for inclusion in a block and returns its hash    |

### Chain

| Method                   | Description                                                                                         |
| ------------------------ | --------------------------------------------------------------------------------------------------- |
| chain\_getBlock          | Returns a full block by its hash, including the header and the scale-encoded extrinsics it contains |
| chain\_getBlockHash      | Returns the block hash for a given block number                                                     |
| chain\_getFinalizedHead  | Returns the hash of the most recently finalized block                                               |
| chain\_getHeader         | Returns the header of a block by its hash, without the extrinsics                                   |
| chain\_getRuntimeVersion | Returns the runtime version at a given block                                                        |

### State

<table data-search="false"><thead><tr><th>Method</th><th>Description</th></tr></thead><tbody><tr><td>state_getRuntimeVersion</td><td>Returns the runtime version at a given block, including the spec name, spec version, and the supported runtime api versions</td></tr><tr><td>state_getMetadata</td><td>Returns the scale-encoded runtime metadata at a given block</td></tr><tr><td>state_getStorage</td><td>Returns the raw scale-encoded value stored at a storage key, at a given block</td></tr><tr><td>state_getStorageHash</td><td>Returns the hash of the value stored at a storage key, at a given block</td></tr><tr><td>state_getStorageSize</td><td>Returns the size in bytes of the value stored at a storage key, at a given block</td></tr><tr><td>state_getKeys</td><td>Returns the storage keys that start with a given prefix, at a given block</td></tr><tr><td>state_getKeysPaged</td><td>Returns a page of storage keys starting from a prefix, bounded by a count and an optional start key</td></tr><tr><td>state_getPairs</td><td>Returns the key/value pairs under a storage prefix, at a given block</td></tr><tr><td>state_queryStorageAt</td><td>Returns the current values for a list of storage keys at a given block, in a single call</td></tr><tr><td>state_call</td><td>Executes a runtime api call by name with scale-encoded arguments and returns the scale-encoded result, at a given block</td></tr><tr><td>state_getReadProof</td><td>Returns a merkle read proof for a set of storage keys at a given block</td></tr><tr><td>state_getChildReadProof</td><td>Returns a merkle read proof for keys in a child trie at a given block</td></tr><tr><td>state_traceBlock</td><td>Re-executes a block and returns storage access and event traces filtered by the given targets, storage keys, and methods</td></tr></tbody></table>

### System

<table data-search="false"><thead><tr><th>Method</th><th>Description</th></tr></thead><tbody><tr><td>system_chain</td><td>Returns the human-readable name of the chain the node is connected to</td></tr><tr><td>system_chainType</td><td>Returns the type of the chain the node is connected to, such as live, local, or development</td></tr><tr><td>system_health</td><td>Returns the health of the node, including its peer count, whether it is syncing, and whether it is expected to have peers</td></tr><tr><td>system_properties</td><td>Returns chain-specific properties, including the token symbol, token decimals, and ss58 address format</td></tr><tr><td>system_version</td><td>Returns the version string of the node's client software</td></tr><tr><td>system_name</td><td>Returns the name of the node's client implementation</td></tr><tr><td>system_nodeRoles</td><td>Returns the roles the node performs on the network, such as full or authority</td></tr><tr><td>system_syncState</td><td>Returns the sync state of the node: the block it started syncing from, the current block, and the highest known block</td></tr><tr><td>system_peers</td><td>Returns details of the peers currently connected to the node</td></tr><tr><td>system_localPeerId</td><td>Returns the base-58 encoded libp2p peer id of the local node</td></tr><tr><td>system_localListenAddresses</td><td>Returns the libp2p multiaddresses the local node is listening on</td></tr><tr><td>system_reservedPeers</td><td>Returns the set of reserved peers configured on the node</td></tr><tr><td>system_dryRun</td><td>Dry-runs an extrinsic against the current state and returns the encoded result without submitting it</td></tr></tbody></table>

### Payment

| Method                   | Description                                                                                                    |
| ------------------------ | -------------------------------------------------------------------------------------------------------------- |
| payment\_queryInfo       | Returns the weight and estimated fee for a submitted extrinsic                                                 |
| payment\_queryFeeDetails | Returns the breakdown of the inclusion fee for an extrinsic: the base fee, length fee, and adjusted weight fee |

### Consensus (GRANDPA & BEEFY)

| Method                  | Description                                                                                                |
| ----------------------- | ---------------------------------------------------------------------------------------------------------- |
| grandpa\_roundState     | Returns the state of the current grandpa voting round, including prevotes and precommits and their weights |
| grandpa\_proveFinality  | Returns a grandpa finality proof for a given block number, if available                                    |
| beefy\_getFinalizedHead | Returns the hash of the latest block finalized by beefy                                                    |

### Child State

| Method                     | Description                                                                                                 |
| -------------------------- | ----------------------------------------------------------------------------------------------------------- |
| childstate\_getKeys        | Returns the keys in a child trie that match a prefix, at a given block                                      |
| childstate\_getKeysPaged   | Returns a page of keys in a child trie starting from a prefix, bounded by a count and an optional start key |
| childstate\_getStorage     | Returns the value stored at a key within a child trie, at a given block                                     |
| childstate\_getStorageHash | Returns the hash of the value stored at a key within a child trie, at a given block                         |
| childstate\_getStorageSize | Returns the size in bytes of the value stored at a key within a child trie, at a given block                |

### MMR

| Method             | Description                                                                                                                         |
| ------------------ | ----------------------------------------------------------------------------------------------------------------------------------- |
| mmr\_root          | Returns the merkle mountain range (mmr) root at a given block                                                                       |
| mmr\_generateProof | Generates an mmr proof for one or more block numbers, returning the leaves and the proof needed to verify them against the mmr root |
| mmr\_verifyProof   | Verifies an mmr proof for a set of leaves against the node's mmr state and returns whether the proof is valid                       |

### RPC

| Method       | Description                                           |
| ------------ | ----------------------------------------------------- |
| rpc\_methods | Returns the list of json-rpc methods the node exposes |

## Support

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Polkadot.js API](https://polkadot.js.org/docs/api/)
* [Polkadot (DOT)](../)
