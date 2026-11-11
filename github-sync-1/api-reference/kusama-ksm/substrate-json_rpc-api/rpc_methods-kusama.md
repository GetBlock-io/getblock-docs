---
description: >-
  Example code for the rpc_methods JSON-RPC method. Complete guide on how to use
  rpc_methods JSON-RPC in GetBlock Web3 documentation.
---

# rpc\_methods - Kusama

Returns the list of JSON-RPC methods the node exposes. Use it to discover which namespaces and methods are available on the endpoint.

{% hint style="info" %}
Available over both the JSON-RPC (HTTP) and WebSocket interfaces.
{% endhint %}

## Parameters

{% hint style="info" %}
This method takes no parameters; send an empty `params` array.
{% endhint %}

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "rpc_methods",
    "params": []
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    id: 'getblock.io',
    method: 'rpc_methods',
    params: []
}, { headers: { 'Content-Type': 'application/json' } });

console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests

response = requests.post(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'id': 'getblock.io',
        'method': 'rpc_methods',
        'params': []
    }
)

print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="example.rs" %}
```rust
use reqwest::Client;
use serde_json::{json, Value};

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    let response = client
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/")
        .json(&json!({
            "jsonrpc": "2.0",
            "id": "getblock.io",
            "method": "rpc_methods",
            "params": []
        }))
        .send().await?
        .json::<Value>().await?;
    println!("{}", response["result"]);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "methods": [
            "account_nextIndex",
            "author_hasKey",
            "author_hasSessionKeys",
            "author_insertKey",
            "author_pendingExtrinsics",
            "author_removeExtrinsic",
            "author_rotateKeys",
            "author_rotateKeysWithOwner",
            "author_submitAndWatchExtrinsic",
            "author_submitExtrinsic",
            "author_unwatchExtrinsic",
            "babe_epochAuthorship",
            "beefy_getFinalizedHead",
            "beefy_subscribeJustifications",
            "beefy_unsubscribeJustifications",
            "bitswap_v1_get",
            "chainHead_v1_body",
            "chainHead_v1_call",
            "chainHead_v1_continue",
            "chainHead_v1_follow",
            "chainHead_v1_header",
            "chainHead_v1_stopOperation",
            "chainHead_v1_storage",
            "chainHead_v1_unfollow",
            "chainHead_v1_unpin",
            "chainSpec_v1_chainName",
            "chainSpec_v1_genesisHash",
            "chainSpec_v1_properties",
            "chain_getBlock",
            "chain_getBlockHash",
            "chain_getFinalisedHead",
            "chain_getFinalizedHead",
            "chain_getHead",
            "chain_getHeader",
            "chain_getRuntimeVersion",
            "chain_subscribeAllHeads",
            "chain_subscribeFinalisedHeads",
            "chain_subscribeFinalizedHeads",
            "chain_subscribeNewHead",
            "chain_subscribeNewHeads",
            "chain_subscribeRuntimeVersion",
            "chain_unsubscribeAllHeads",
            "chain_unsubscribeFinalisedHeads",
            "chain_unsubscribeFinalizedHeads",
            "chain_unsubscribeNewHead",
            "chain_unsubscribeNewHeads",
            "chain_unsubscribeRuntimeVersion",
            "childstate_getKeys",
            "childstate_getKeysPaged",
            "childstate_getKeysPagedAt",
            "childstate_getStorage",
            "childstate_getStorageEntries",
            "childstate_getStorageHash",
            "childstate_getStorageSize",
            "grandpa_proveFinality",
            "grandpa_roundState",
            "grandpa_subscribeJustifications",
            "grandpa_unsubscribeJustifications",
            "mmr_generateAncestryProof",
            "mmr_generateProof",
            "mmr_root",
            "mmr_verifyProof",
            "mmr_verifyProofStateless",
            "offchain_localStorageClear",
            "offchain_localStorageGet",
            "offchain_localStorageSet",
            "payment_queryFeeDetails",
            "payment_queryInfo",
            "rpc_methods",
            "state_call",
            "state_callAt",
            "state_getChildReadProof",
            "state_getKeys",
            "state_getKeysPaged",
            "state_getKeysPagedAt",
            "state_getMetadata",
            "state_getPairs",
            "state_getReadProof",
            "state_getRuntimeVersion",
            "state_getStorage",
            "state_getStorageAt",
            "state_getStorageHash",
            "state_getStorageHashAt",
            "state_getStorageSize",
            "state_getStorageSizeAt",
            "state_queryStorage",
            "state_queryStorageAt",
            "state_subscribeRuntimeVersion",
            "state_subscribeStorage",
            "state_traceBlock",
            "state_trieMigrationStatus",
            "state_unsubscribeRuntimeVersion",
            "state_unsubscribeStorage",
            "subscribe_newHead",
            "sync_state_genSyncSpec",
            "system_accountNextIndex",
            "system_addLogFilter",
            "system_addReservedPeer",
            "system_chain",
            "system_chainType",
            "system_dryRun",
            "system_dryRunAt",
            "system_health",
            "system_localListenAddresses",
            "system_localPeerId",
            "system_name",
            "system_nodeRoles",
            "system_peers",
            "system_properties",
            "system_removeReservedPeer",
            "system_reservedPeers",
            "system_resetLogFilter",
            "system_syncState",
            "system_unstable_networkState",
            "system_version",
            "transactionWatch_v1_submitAndWatch",
            "transactionWatch_v1_unwatch",
            "transaction_v1_broadcast",
            "transaction_v1_stop",
            "unsubscribe_newHead"
        ]
    }
}
```

## Response Fields

| Field   | Type  | Description                                                    |
| ------- | ----- | -------------------------------------------------------------- |
| methods | array | Alphabetical list of every JSON-RPC method the endpoint serves |

## Use Cases

* **Capability Discovery**: Detect which methods the endpoint serves
* **Feature Gating**: Enable features based on available methods
* **Diagnostics**: Confirm a method exists before calling it

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| -32603 / Internal error   | Internal error | The node failed to list methods                   |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
