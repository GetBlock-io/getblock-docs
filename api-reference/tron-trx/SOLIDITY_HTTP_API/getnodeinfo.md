---
description: >-
  Get node details with getnodeinfo in Tron's HTTP REST API Interface. Retrieve
  network info, version, and more.
---

# getnodeinfo - TRON

## Description

The getnodeinfo HTTP REST API method in the Tron protocol provides essential node details, including network status, version, and connectivity metrics. As part of the HTTP REST API interface, 'getnodeinfo Web3' enables developers to monitor node health and performance programmatically. This method is critical for debugging, analytics, and ensuring seamless integration with the Tron blockchain. Use getnodeinfo REST protocol to fetch real-time data like peer count, block height, and sync status, optimizing decentralized application (dApp) functionality. Ideal for node operators and developers leveraging Tron's Web3 ecosystem.

## Supported Networks

The getnodeinfo HTTP REST API method supports the following network types

* **Mainnet**
* **Testnets**

## Parameters

None: This method does not require any parameters.

### URL

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

Here’s a sample cURL request using getnodeinfo

Request

```json
curl --request GET \
     --url https://go.getblock.io/<ACCESS-TOKEN>/walletsolidity/getnodeinfo \
     --header 'accept: application/json'
```

Response

```json

  "activeConnectCount": 9,
  "beginSyncNum": 71553984,
  "block": "Num:71554003,ID:000000000443d3d306dcc0767941b056f65f884d6e20e119dea455074027e58e",
  "cheatWitnessInfoMap": {},
  "configNodeInfo": {
    "activeNodeSize": 0,
    "allowAdaptiveEnergy": 0,
    "allowCreationOfContracts": 0,
    "backupListenPort": 10001,
    "backupMemberSize": 0,
    "backupPriority": 8,
    "codeVersion": "4.7.7",
    "dbVersion": 2,
    "discoverEnable": true,
    "listenPort": 18888,
    "maxConnectCount": 30,
    "maxTimeRatio": 50.0,
    "minParticipationRate": 15,
    "minTimeRatio": 0.0,
    "p2pVersion": "11111",
    "passiveNodeSize": 0,
    "sameIpMaxConnectCount": 2,
    "sendNodeSize": 59,
    "supportConstant": true,
    "versionNum": "18386"
  },
  "currentConnectCount": 9,
  "machineInfo": {
    "cpuCount": 20,
    "cpuRate": 0.1111111111111111,
    "deadLockThreadCount": 0,
    "deadLockThreadInfoList": [],
    "freeMemory": 520982528,
    "javaVersion": "1.8.0_202",
    "jvmFreeMemory": 10589297000,
    "jvmTotalMemory": 25638993920,
    "memoryDescInfoList": [
      {
        "initSize": 2555904,
        "maxSize": 268435456,
        "name": "Code Cache",
        "useRate": 0.30190515518188477,
        "useSize": 81042048
      },
      {
        "initSize": 0,
        "maxSize": -1,
        "name": "Metaspace",
        "useRate": 0.9344504140952492,
        "useSize": 82176616
      },
      {
        "initSize": 0,
        "maxSize": 1073741824,
        "name": "Compressed Class Space",
        "useRate": 0.008216939866542816,
        "useSize": 8822872
      },
      {
        "initSize": 1047003136,
        "maxSize": 1047003136,
        "name": "Par Eden Space",
        "useRate": 0.9536129908974791,
        "useSize": 998435792
      },
      {
        "initSize": 130809856,
        "maxSize": 130809856,
        "name": "Par Survivor Space",
        "useRate": 0.22430927528885897,
        "useSize": 29341864
      },
      {
        "initSize": 24461180928,
        "maxSize": 24461180928,
        "name": "CMS Old Gen",
        "useRate": 0.573231493004065,
        "useSize": 14021919264
      }
    ],
    "osName": "Linux 5.10.0-34-amd64",
    "processCpuRate": 0.05555555555555555,
    "threadCount": 302,
    "totalMemory": 67075801088
  },
  "passiveConnectCount": 0,
  "peerList": [
    {
      "active": true,
      "avgLatency": 13.0,
      "blockInPorcSize": 0,
      "connectTime": 1745225705375,
      "disconnectTimes": 1,
      "headBlockTimeWeBothHave": 0,
      "headBlockWeBothHave": "Num:71554021,ID:000000000443d3e516d960e488a67d3724329359cf13a1860358bd9ea6e21b0b",
      "host": "/188.40.83.153",
      "inFlow": 0,
      "lastBlockUpdateTime": 1745316981360,
      "lastSyncBlock": "",
      "localDisconnectReason": "TIME_OUT",
      "needSyncFromPeer": false,
      "needSyncFromUs": false,
      "nodeCount": 96,
      "nodeId": "9425a45639ddbfb5160d7829c294b52ae161a544e4a94930ce4dc3b4740d39bc9adf95c55e874705967331599779f53a5c8a537a835fb6bae4c7514204d83013",
      "port": 18888,
      "remainNum": 0,
      "remoteDisconnectReason": "",
      "score": 0,
      "syncBlockRequestedSize": 0,
      "syncFlag": false,
      "syncToFetchSize": 0,
      "syncToFetchSizePeekNum": -1,
      "unFetchSynNum": 0
    },
    {
      "active": true,
      "avgLatency": 44.0,
      "blockInPorcSize": 0,
      "connectTime": 1745188896185,
      "disconnectTimes": 0,
      "headBlockTimeWeBothHave": 0,
      "headBlockWeBothHave": "Num:71554021,ID:000000000443d3e516d960e488a67d3724329359cf13a1860358bd9ea6e21b0b",
      "host": "/65.109.20.30",
      "inFlow": 0,
      "lastBlockUpdateTime": 1745316981360,
      "lastSyncBlock": "",
      "localDisconnectReason": "",
      "needSyncFromPeer": false,
      "needSyncFromUs": false,
      "nodeCount": 96,
      "nodeId": "9425a45639ddbfb5160d7829c294b52ae161a544e4a94930ce4dc3b4740d39bc9adf95c55e874705967331599779f53a5c8a537a835fb6bae4c7514204d83013",
      "port": 18888,
      "remainNum": 0,
      "remoteDisconnectReason": "",
      "score": 0,
      "syncBlockRequestedSize": 0,
      "syncFlag": false,
      "syncToFetchSize": 0,
      "syncToFetchSizePeekNum": -1,
      "unFetchSynNum": 0
    },
    {
      "active": true,
      "avgLatency": 45.0,
      "blockInPorcSize": 0,
      "connectTime": 1745310655160,
      "disconnectTimes": 2,
      "headBlockTimeWeBothHave": 0,
      "headBlockWeBothHave": "Num:71554021,ID:000000000443d3e516d960e488a67d3724329359cf13a1860358bd9ea6e21b0b",
      "host": "/34.118.95.84",
      "inFlow": 0,
      "lastBlockUpdateTime": 1745316981360,
      "lastSyncBlock": "Num:71552382,ID:000000000443cd7e07c539728d742b27636c2ae7b5777fcb822176511005d2ca",
      "localDisconnectReason": "TIME_OUT",
      "needSyncFromPeer": false,
      "needSyncFromUs": false,
      "nodeCount": 96,
      "nodeId": "9425a45639ddbfb5160d7829c294b52ae161a544e4a94930ce4dc3b4740d39bc9adf95c55e874705967331599779f53a5c8a537a835fb6bae4c7514204d83013",
      "port": 18888,
      "remainNum": 0,
      "remoteDisconnectReason": "TIME_OUT",
      "score": 0,
      "syncBlockRequestedSize": 0,
      "syncFlag": false,
      "syncToFetchSize": 0,
      "syncToFetchSizePeekNum": -1,
      "unFetchSynNum": 0
    },
    {
      "active": true,
      "avgLatency": 224.0,
      "blockInPorcSize": 0,
      "connectTime": 1745309516198,
      "disconnectTimes": 6,
      "headBlockTimeWeBothHave": 0,
      "headBlockWeBothHave": "Num:71554019,ID:000000000443d3e312379afed801c7c4debd883f264f3ae1e0056931311135d5",
      "host": "/34.30.250.219",
      "inFlow": 0,
      "lastBlockUpdateTime": 1745316975750,
      "lastSyncBlock": "",
      "localDisconnectReason": "TIME_OUT",
      "needSyncFromPeer": false,
      "needSyncFromUs": false,
      "nodeCount": 96,
      "nodeId": "9425a45639ddbfb5160d7829c294b52ae161a544e4a94930ce4dc3b4740d39bc9adf95c55e874705967331599779f53a5c8a537a835fb6bae4c7514204d83013",
      "port": 18888,
      "remainNum": 0,
      "remoteDisconnectReason": "TIME_OUT",
      "score": 0,
      "syncBlockRequestedSize": 0,
      "syncFlag": false,
      "syncToFetchSize": 0,
      "syncToFetchSizePeekNum": -1,
      "unFetchSynNum": 0
    },
    {
      "active": true,
      "avgLatency": 248.0,
      "blockInPorcSize": 0,
      "connectTime": 1745189006834,
      "disconnectTimes": 0,
      "headBlockTimeWeBothHave": 0,
      "headBlockWeBothHave": "Num:71554014,ID:000000000443d3de9910f6d150e81e5ce27a437cdc1e2d4055018e6d2d55a563",
      "host": "/45.137.213.115",
      "inFlow": 0,
      "lastBlockUpdateTime": 1745316960970,
      "lastSyncBlock": "",
      "localDisconnectReason": "",
      "needSyncFromPeer": false,
      "needSyncFromUs": false,
      "nodeCount": 96,
      "nodeId": "9425a45639ddbfb5160d7829c294b52ae161a544e4a94930ce4dc3b4740d39bc9adf95c55e874705967331599779f53a5c8a537a835fb6bae4c7514204d83013",
      "port": 18888,
      "remainNum": 0,
      "remoteDisconnectReason": "",
      "score": 0,
      "syncBlockRequestedSize": 0,
      "syncFlag": false,
      "syncToFetchSize": 0,
      "syncToFetchSizePeekNum": -1,
      "unFetchSynNum": 0
    },
    {
      "active": true,
      "avgLatency": 295.0,
      "blockInPorcSize": 0,
      "connectTime": 1745199542324,
      "disconnectTimes": 0,
      "headBlockTimeWeBothHave": 0,
      "headBlockWeBothHave": "Num:71554019,ID:000000000443d3e312379afed801c7c4debd883f264f3ae1e0056931311135d5",
      "host": "/101.44.25.152",
      "inFlow": 0,
      "lastBlockUpdateTime": 1745316975750,
      "lastSyncBlock": "",
      "localDisconnectReason": "",
      "needSyncFromPeer": false,
      "needSyncFromUs": false,
      "nodeCount": 96,
      "nodeId": "9425a45639ddbfb5160d7829c294b52ae161a544e4a94930ce4dc3b4740d39bc9adf95c55e874705967331599779f53a5c8a537a835fb6bae4c7514204d83013",
      "port": 18888,
      "remainNum": 0,
      "remoteDisconnectReason": "",
      "score": 0,
      "syncBlockRequestedSize": 0,
      "syncFlag": false,
      "syncToFetchSize": 0,
      "syncToFetchSizePeekNum": -1,
      "unFetchSynNum": 0
    },
    {
      "active": true,
      "avgLatency": 300.0,
      "blockInPorcSize": 0,
      "connectTime": 1745311664886,
      "disconnectTimes": 0,
      "headBlockTimeWeBothHave": 0,
      "headBlockWeBothHave": "Num:71554019,ID:000000000443d3e312379afed801c7c4debd883f264f3ae1e0056931311135d5",
      "host": "/165.227.240.58",
      "inFlow": 0,
      "lastBlockUpdateTime": 1745316975750,
      "lastSyncBlock": "Num:71553125,ID:000000000443d065bdb9ab890406364b133bea08834bd5d18291cc243e4b07f0",
      "localDisconnectReason": "",
      "needSyncFromPeer": false,
      "needSyncFromUs": false,
      "nodeCount": 96,
      "nodeId": "9425a45639ddbfb5160d7829c294b52ae161a544e4a94930ce4dc3b4740d39bc9adf95c55e874705967331599779f53a5c8a537a835fb6bae4c7514204d83013",
      "port": 18888,
      "remainNum": 0,
      "remoteDisconnectReason": "",
      "score": 0,
      "syncBlockRequestedSize": 0,
      "syncFlag": false,
      "syncToFetchSize": 0,
      "syncToFetchSizePeekNum": -1,
      "unFetchSynNum": 0
    },
    {
      "active": true,
      "avgLatency": 406.0,
      "blockInPorcSize": 0,
      "connectTime": 1745316428983,
      "disconnectTimes": 7,
      "headBlockTimeWeBothHave": 0,
      "headBlockWeBothHave": "Num:71554015,ID:000000000443d3df9cb612096ccfc075544474f3959fb3c71e74a1f5fa906fce",
      "host": "/34.151.237.76",
      "inFlow": 0,
      "lastBlockUpdateTime": 1745316963790,
      "lastSyncBlock": "Num:71553837,ID:000000000443d32d303be7963974a144dae68ccaa1c22301c3d66504c720494a",
      "localDisconnectReason": "BAD_PROTOCOL",
      "needSyncFromPeer": false,
      "needSyncFromUs": false,
      "nodeCount": 96,
      "nodeId": "9425a45639ddbfb5160d7829c294b52ae161a544e4a94930ce4dc3b4740d39bc9adf95c55e874705967331599779f53a5c8a537a835fb6bae4c7514204d83013",
      "port": 18888,
      "remainNum": 0,
      "remoteDisconnectReason": "TIME_OUT",
      "score": 0,
      "syncBlockRequestedSize": 0,
      "syncFlag": false,
      "syncToFetchSize": 0,
      "syncToFetchSizePeekNum": -1,
      "unFetchSynNum": 0
    },
    {
      "active": true,
      "avgLatency": 551.0,
      "blockInPorcSize": 0,
      "connectTime": 1745312511619,
      "disconnectTimes": 0,
      "headBlockTimeWeBothHave": 0,
      "headBlockWeBothHave": "Num:71554019,ID:000000000443d3e312379afed801c7c4debd883f264f3ae1e0056931311135d5",
      "host": "/170.64.249.239",
      "inFlow": 0,
      "lastBlockUpdateTime": 1745316975750,
      "lastSyncBlock": "",
      "localDisconnectReason": "",
      "needSyncFromPeer": false,
      "needSyncFromUs": false,
      "nodeCount": 96,
      "nodeId": "9425a45639ddbfb5160d7829c294b52ae161a544e4a94930ce4dc3b4740d39bc9adf95c55e874705967331599779f53a5c8a537a835fb6bae4c7514204d83013",
      "port": 18888,
      "remainNum": 0,
      "remoteDisconnectReason": "",
      "score": 0,
      "syncBlockRequestedSize": 0,
      "syncFlag": false,
      "syncToFetchSize": 0,
      "syncToFetchSizePeekNum": -1,
      "unFetchSynNum": 0
    }
  ],
  "solidityBlock": "Num:71553985,ID:000000000443d3c193034604aa290d600ba8eabb175777d996e30532b10debe8",
  "totalFlow": 0
}
```

## Body Parameters

* **beginSyncNum** (`int64`):\
  The block number from which the node started syncing.
* **block** (`string`):\
  Information about the latest block, including its height and block ID.
* **solidityBlock** (`string`):\
  Information about the latest solidified (irreversible) block, including its height and block ID.
* **currentConnectCount** (`int32`):\
  The total number of current connections to other nodes.
* **activeConnectCount** (`int32`):\
  The number of active (outbound) node connections.
* **passiveConnectCount** (`int32`):\
  The number of passive (inbound) node connections.
* **totalFlow** (`int64`):\
  The total amount of TCP network traffic handled by the node.
* **peerInfoList** (`PeerInfo[]`):\
  A list of connected peer nodes.\
  For detailed structure and fields, refer to the corresponding `PeerInfo` protobuf definition.
* **configNodeInfo** (`ConfigNodeInfo`):\
  The configuration details of the node.\
  See the `ConfigNodeInfo` protobuf definition for more information.
* **machineInfo** (`MachineInfo`):\
  Information about the machine where the node is running.\
  Detailed structure is available in the `MachineInfo` protobuf definition.
* **cheatWitnessInfoMap** (`map<string, string>`):\
  A mapping containing information about super representatives (SRs) that are suspected of cheating.

## Use Case

Here are some use-cases for the `getnodeinfo` method in Web3 programming:

1. **Network Diagnostics and Monitoring**\
   Developers can use `getnodeinfo` to retrieve essential details about a node's status, such as its version, protocol, and connectivity. This is useful for debugging network issues, ensuring the node is synced, or verifying compatibility with other nodes in the decentralized network.
2. **Node Management and Configuration**\
   The method helps administrators or automated scripts gather node metadata (e.g., peer count, chain ID) to optimize performance. For example, it can inform load balancing decisions or trigger alerts if the node falls behind the latest block height.
3. **User Transparency and Trust**\
   DApps (decentralized applications) might display node information to users—like the client software version—to build trust by proving the backend operates on a legitimate, up-to-date network participant.

_(Note: The provided JSON `{"value": "62747474657374"}` appears unrelated to `getnodeinfo` and may represent hex-encoded data, but further context is needed to interpret it.)_

## Code for getnodeinfo

```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/walletsolidity/getnodeinfo"
headers = {"accept": "application/json"}

response = requests.get(url, headers=headers)
print(response.json())
```

## Common Errors

**Common Errors**\
When using the `getnodeinfo` HTTP Tron method, the following issues may occur:

* **Invalid Node Configuration**: The node may not be properly synced or configured, returning incomplete data. Ensure the node is fully synced and the API is enabled in the configuration file.
* **Network Connectivity Issues**: Timeouts or failed responses can occur due to unstable network connections. Verify your internet connection and check the node’s accessibility via ping or curl.
* **Rate Limiting or Throttling**: Excessive requests may trigger rate limits, resulting in temporary bans. Space out requests or use a load-balanced setup for high-frequency queries.

The `getnodeinfo` method is invaluable for Web3 apps, providing real-time node status and network health metrics. It enables developers to monitor node performance, ensure reliability, and optimize interactions with the Tron blockchain.

### conclusion

Here’s a concise conclusion incorporating your requested keywords:

The provided HTTP response with the value `"62747474657374"` appears to be a hexadecimal string, which could relate to a Tron blockchain transaction or node data. For further details, you might use the **getnodeinfo REST** method in **Tron** to retrieve node-specific information, such as network status or peer details. Integrating **getnodeinfo** into your workflow can help monitor node health and ensure seamless interactions with the **Tron** network. Always validate such data against official **Tron** documentation for accuracy.
