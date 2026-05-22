---
description: >-
  Example code for the rpc_methods JSON-RPC method. Complete guide on how to use
  rpc_methods JSON-RPC in GetBlock Web3 documentation.
---

# rpc\_methods - Midnight

This method returns the list of all RPC methods exposed by the connected node. Use it to confirm which methods are available before relying on them.

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "rpc_methods",
    "params": [],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "rpc_methods",
    "params": [],
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers: {
        'Content-Type': 'application/json'
    },
    data: data
};

axios(config)
    .then(response => console.log(JSON.stringify(response.data, null, 2)))
    .catch(error => console.log(error));
```
{% endtab %}

{% tab title="Python (Requests)" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "rpc_methods",
    "params": [],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endtab %}

{% tab title="Rust (Reqwest)" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let payload = json!({
        "jsonrpc": "2.0",
        "method": "rpc_methods",
        "params": [],
        "id": "getblock.io"
});

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&payload)
        .send()
        .await?;

    let result: serde_json::Value = response.json().await?;
    println!("{:#?}", result);

    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "result": {
        "methods": [
            "account_nextIndex",
            "archive_v1_body",
            "archive_v1_call",
            "archive_v1_finalizedHeight",
            "archive_v1_genesisHash",
            "archive_v1_hashByHeight",
            "archive_v1_header",
            "archive_v1_stopStorage",
            "archive_v1_storage",
            "archive_v1_storageDiff",
            "archive_v1_storageDiff_stopStorageDiff",
            "author_hasKey",
            "author_hasSessionKeys",
            "author_insertKey",
            "author_pendingExtrinsics",
            "author_removeExtrinsic",
            "author_rotateKeys",
            "author_submitAndWatchExtrinsic",
            "author_submitExtrinsic",
            "author_unwatchExtrinsic",
            "beefy_getFinalizedHead",
            "beefy_subscribeJustifications",
            "beefy_unsubscribeJustifications",
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
            "midnight_apiVersions",
            "midnight_contractState",
            "midnight_ledgerStateRoot",
            "midnight_ledgerVersion",
            "midnight_zswapStateRoot",
            "mmr_generateProof",
            "mmr_root",
            "mmr_verifyProof",
            "mmr_verifyProofStateless",
            "network_peerReputation",
            "network_peerReputations",
            "network_unbanPeer",
            "offchain_localStorageClear",
            "offchain_localStorageGet",
            "offchain_localStorageSet",
            "rpc_methods",
            "sidechain_getAriadneParameters",
            "sidechain_getEpochCommittee",
            "sidechain_getParams",
            "sidechain_getRegistrations",
            "sidechain_getStatus",
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
            "state_unsubscribeRuntimeVersion",
            "state_unsubscribeStorage",
            "subscribe_newHead",
            "systemParameters_getAriadneParameters",
            "systemParameters_getDParameter",
            "systemParameters_getTermsAndConditions",
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
    },
    "id": "getblock.io"
}

```
{% endcode %}

## Response Parameters

| Field          | Type            | Description                                     |
| -------------- | --------------- | ----------------------------------------------- |
| result.version | integer         | Version of the methods listing format           |
| result.methods | array of string | Names of all RPC methods supported by this node |

## Use Cases

* Capability detection before calling potentially-unavailable methods
* Client libraries that adapt to the node's actual surface
* Auditing what an RPC provider exposes

## Error Handling

| Status Code | Error Message     | Cause                                       |
| ----------- | ----------------- | ------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`         |
| -32602      | Invalid params    | Request parameters are missing or malformed |
| 429         | Too Many Requests | Rate limit exceeded for your plan           |

## SDK Integration

{% tabs %}
{% tab title="@polkadot/api (JavaScript)" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const wsProvider = new WsProvider('wss://go.getblock.io/<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider: wsProvider });

// Call via the typed API:
const result = await api.rpc.rpc.methods();
console.log(result.toJSON());

// Or call the raw JSON-RPC method:
const raw = await api.rpc.rpc.methods();
// const raw = await (api as any)._rpcCore.provider.send('rpc_methods', []);
```
{% endtab %}

{% tab title="substrate-interface (Python)" %}
```python
from substrateinterface import SubstrateInterface

substrate = SubstrateInterface(url='wss://go.getblock.io/<ACCESS-TOKEN>/')

# Generic RPC call:
result = substrate.rpc_request('rpc_methods', [])
print(result)
```
{% endtab %}
{% endtabs %}
