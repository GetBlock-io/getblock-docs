# getnodeinfo


## Meta Description
Discover 'getnodeinfo' using Tron’s RESTful API Interface for seamless node information retrieval.

## Description
The 'getnodeinfo' Web3 method in the Tron protocol provides a comprehensive overview of node details via the RESTful API Interface. This method is an essential tool for developers and network administrators seeking to monitor and manage nodes efficiently. By leveraging the 'getnodeinfo RPC protocol', users can retrieve vital information such as the node's status, version, and network connectivity. The technical design of this method ensures a user-friendly experience, offering reliable data critical for maintaining network integrity and performance. Whether you're integrating with Tron’s blockchain or conducting routine checks, 'getnodeinfo' delivers precise and actionable insights to support your development and operational needs.

## Supported Networks
The getnodeinfo REST API method supports the following network types
- **Mainnet**
- **Testnets**

## Parameters

None: This method does not require any parameters.

### URL
```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
Here’s a sample cURL request using getnodeinfo

Request
```json
curl --request POST 
     --url https://go.getblock.io/<ACCESS-TOKEN>/ \
     --header 'accept: application/json' 
     --header 'content-type: application/json' 
     --data {
  "address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
  "visible": true
}
```

Response
```json

{
  "activeConnectCount": 9,
  "beginSyncNum": 71562174,
  "block": "Num:71562193,ID:000000000443f3d1b185131a08abaa5c1c1b148f3c4d46f117d732afb9076a6c",
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
    "cpuRate": 0.13043478260869565,
    "deadLockThreadCount": 0,
    "deadLockThreadInfoList": [],
    "freeMemory": 660168704,
    "javaVersion": "1.8.0_202",
    "jvmFreeMemory": 20474562280,
    "jvmTotalMemory": 25638993920,
    "memoryDescInfoList": [
      {
        "initSize": 2555904,
        "maxSize": 268435456,
        "name": "Code Cache",
        "useRate": 0.3024313449859619,
        "useSize": 81183296
      },
      {
        "initSize": 0,
        "maxSize": -1,
        "name": "Metaspace",
        "useRate": 0.9334216021756292,
        "useSize": 82330832
      },
      {
        "initSize": 0,
        "maxSize": 1073741824,
        "name": "Compressed Class Space",
        "useRate": 0.008230403065681458,
        "useSize": 8837328
      },
      {
        "initSize": 1047003136,
        "maxSize": 1047003136,
        "name": "Par Eden Space",
        "useRate": 0.5804656080800888,
        "useSize": 607749312
      },
      {
        "initSize": 130809856,
        "maxSize": 130809856,
        "name": "Par Survivor Space",
        "useRate": 0.8560315210499123,
        "useSize": 111977360
      },
      {
        "initSize": 24461180928,
        "maxSize": 24461180928,
        "name": "CMS Old Gen",
        "useRate": 0.18170443124077776,
        "useSize": 4444704968
      }
    ],
    "osName": "Linux 5.10.0-34-amd64",
    "processCpuRate": 0.11956521739130435,
    "threadCount": 303,
    "totalMemory": 67075801088
  },
  "passiveConnectCount": 0,
  "peerList": [
    {
      "active": true,
      "avgLatency": 12.0,
      "blockInPorcSize": 0,
      "connectTime": 1745333487937,
      "disconnectTimes": 0,
      "headBlockTimeWeBothHave": 0,
      "headBlockWeBothHave": "Num:71562191,ID:000000000443f3cf59823924ea3d7a97e26fb73022aa8cb69f45518cef03f02b",
      "host": "/168.119.5.80",
      "inFlow": 0,
      "lastBlockUpdateTime": 1745341497527,
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
      "avgLatency": 13.0,
      "blockInPorcSize": 0,
      "connectTime": 1745225705375,
      "disconnectTimes": 1,
      "headBlockTimeWeBothHave": 0,
      "headBlockWeBothHave": "Num:71562193,ID:000000000443f3d1b185131a08abaa5c1c1b148f3c4d46f117d732afb9076a6c",
      "host": "/188.40.83.153",
      "inFlow": 0,
      "lastBlockUpdateTime": 1745341503799,
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
      "headBlockWeBothHave": "Num:71562193,ID:000000000443f3d1b185131a08abaa5c1c1b148f3c4d46f117d732afb9076a6c",
      "host": "/65.109.20.30",
      "inFlow": 0,
      "lastBlockUpdateTime": 1745341503799,
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
      "avgLatency": 64.0,
      "blockInPorcSize": 0,
      "connectTime": 1745327203652,
      "disconnectTimes": 3,
      "headBlockTimeWeBothHave": 0,
      "headBlockWeBothHave": "Num:71562192,ID:000000000443f3d04bc09148f84085fcaa7577dcb13438bd884091b8a0467d1c",
      "host": "/34.118.95.84",
      "inFlow": 0,
      "lastBlockUpdateTime": 1745341500605,
      "lastSyncBlock": "Num:71561955,ID:000000000443f2e3083d4650a0cfa3268d97ed22dcc6970533859d81154b3587",
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
      "connectTime": 1745325097703,
      "disconnectTimes": 7,
      "headBlockTimeWeBothHave": 0,
      "headBlockWeBothHave": "Num:71562193,ID:000000000443f3d1b185131a08abaa5c1c1b148f3c4d46f117d732afb9076a6c",
      "host": "/34.30.250.219",
      "inFlow": 0,
      "lastBlockUpdateTime": 1745341503799,
      "lastSyncBlock": "Num:71559559,ID:000000000443e98789a90dabf97c37838c7428bc79bbb49a8d37bc5ef7a68f69",
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
      "headBlockWeBothHave": "Num:71562191,ID:000000000443f3cf59823924ea3d7a97e26fb73022aa8cb69f45518cef03f02b",
      "host": "/45.137.213.115",
      "inFlow": 0,
      "lastBlockUpdateTime": 1745341497527,
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
      "connectTime": 1745333365376,
      "disconnectTimes": 0,
      "headBlockTimeWeBothHave": 0,
      "headBlockWeBothHave": "Num:71562193,ID:000000000443f3d1b185131a08abaa5c1c1b148f3c4d46f117d732afb9076a6c",
      "host": "/101.44.25.152",
      "inFlow": 0,
      "lastBlockUpdateTime": 1745341503799,
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
      "headBlockWeBothHave": "Num:71562193,ID:000000000443f3d1b185131a08abaa5c1c1b148f3c4d46f117d732afb9076a6c",
      "host": "/165.227.240.58",
      "inFlow": 0,
      "lastBlockUpdateTime": 1745341503799,
      "lastSyncBlock": "Num:71560362,ID:000000000443ecaaf0266dc6c4660b2fe18a8772b43229c381c3010d6899a201",
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
      "avgLatency": 551.0,
      "blockInPorcSize": 0,
      "connectTime": 1745312511619,
      "disconnectTimes": 0,
      "headBlockTimeWeBothHave": 0,
      "headBlockWeBothHave": "Num:71562193,ID:000000000443f3d1b185131a08abaa5c1c1b148f3c4d46f117d732afb9076a6c",
      "host": "/170.64.249.239",
      "inFlow": 0,
      "lastBlockUpdateTime": 1745341503799,
      "lastSyncBlock": "Num:71558786,ID:000000000443e68288b24b20567027ecfd49fcbd93b6c1e6b110547b999de780",
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
  "solidityBlock": "Num:71562175,ID:000000000443f3bf504204d8b0bc311e9d30a7934d1fb802cddab9099cfd9c0f",
  "totalFlow": 0
}
```
## Body Parameters

Here is the list of body parameters for the `getnodeinfo` method:

1. **activeConnectCount**: The number of currently active connections.
2. **beginSyncNum**: The block number from which synchronization begins.
3. **block**: Information about the current block, including its number and ID.
4. **cheatWitnessInfoMap**: A map containing information about any cheating witnesses (if any).
5. **configNodeInfo**: Configuration details of the node, including:
   - **activeNodeSize**: The number of active nodes.
   - **allowAdaptiveEnergy**: Whether adaptive energy is allowed.
   - **allowCreationOfContracts**: Whether the creation of contracts is allowed.
   - **backupListenPort**: The port for backup listening.
   - **backupMemberSize**: The size of the backup member.
   - **backupPriority**: The priority level for backups.
   - **codeVersion**: The version of the code running on the node.
   - **dbVersion**: The database version.
   - **discoverEnable**: Whether node discovery is enabled.
   - **listenPort**: The port on which the node is listening.
   - **maxConnectCount**: The maximum number of connections allowed.
   - **maxTimeRatio**: The maximum time ratio.
   - **minParticipationRate**: The minimum participation rate.
   - **minTimeRatio**: The minimum time ratio.
   - **p2pVersion**: The version of the peer-to-peer protocol.
   - **passiveNodeSize**: The number of passive nodes.
   - **sameIpMaxConnectCount**: The maximum number of connections from the same IP.
   - **sendNodeSize**: The size of the sending node.
   - **supportConstant**: Whether constant support is enabled.
   - **versionNum**: The version number of the node.
6. **currentConnectCount**: The current number of connections.
7. **machineInfo**: Information about the machine running the node, including:
   - **cpuCount**: The number of CPU cores.
   - **cpuRate**: The CPU usage rate.
   - **deadLockThreadCount**: The number of deadlock threads.
   - **deadLockThreadInfoList**: A list of information about deadlock threads.
   - **freeMemory**: The amount of free memory available.
   - **javaVersion**: The version of Java being used.
   - **jvmFreeMemory**: The amount of free memory in the JVM.
   - **jvmTotalMemory**: The total memory available to the JVM.
   - **memoryDescInfoList**: A list of memory descriptions, including:
     - **initSize**: Initial size of the memory area.
     - **maxSize**: Maximum size of the memory area.
     - **name**: Name of the memory area.
     - **useRate**: Usage rate of the memory area.
     - **useSize**: Used size of the memory area.
   - **osName**: The name of the operating system.
   - **processCpuRate**: The CPU usage rate of the process.
   - **threadCount**: The number of threads.
   - **totalMemory**: The total memory available on the machine.
8. **passiveConnectCount**: The number of passive connections.
9. **peerList**: A list of peers connected to the node, each with details such as:
   - **active**: Whether the connection is active.
   - **avgLatency**: The average latency.
   - **blockInPorcSize**: The size of the block in processing.
   - **connectTime**: The time the connection was established.
   - **disconnectTimes**: The number of times the connection was disconnected.
   - **headBlockTimeWeBothHave**: The head block time both nodes have.
   - **headBlockWeBothHave**: The head block both nodes have.
   - **host**: The host address of the peer.
   - **inFlow**: The incoming flow.
   - **lastBlockUpdateTime**: The time the last block was updated.
   - **lastSyncBlock**: The last synchronized block.
   - **localDisconnectReason**: The reason for local disconnection.
   - **needSyncFromPeer**: Whether synchronization is needed from the peer.
   - **needSyncFromUs**: Whether synchronization is needed from us.
   - **nodeCount**: The number of nodes.
   - **nodeId**: The node ID.
   - **port**: The port number.
   - **remainNum**: The remaining number.
   - **remoteDisconnectReason**: The reason for remote disconnection.
   - **score**: The score of the peer.
   - **syncBlockRequestedSize**: The requested size of the sync block.
   - **syncFlag**: The synchronization flag.
   - **syncToFetchSize**: The size to fetch for synchronization.
   - **syncToFetchSizePeekNum**: The peak number of the size to fetch for synchronization.
   - **unFetchSynNum**: The number of unfetched syncs.
10. **solidityBlock**: Information about the solidity block, including its number and ID.
11. **totalFlow**: The total flow of data.

## Use Case

Here are some use-cases for the `getnodeinfo` method in Web3 programming:

1. **Network Monitoring and Diagnostics**: The `getnodeinfo` method can be used to monitor the health and status of a blockchain node. By retrieving information such as the node's current block, peers, and protocol version, developers and network administrators can ensure that the node is functioning correctly and is properly synchronized with the rest of the network. This is crucial for maintaining the reliability and efficiency of decentralized applications (dApps) that depend on real-time blockchain data.

2. **Node Management and Optimization**: Developers can leverage the `getnodeinfo` method to gather insights into node performance and resource usage. This information can help in optimizing node configurations, such as adjusting peer connections or upgrading hardware resources, to improve transaction processing speeds and overall network performance. Efficient node management is essential for scaling applications and ensuring seamless user experiences.

3. **Security and Compliance Auditing**: By using the `getnodeinfo` method, organizations can perform security audits and compliance checks on their blockchain nodes. The method provides details about the node's network connections and software versions, which can be analyzed to identify potential vulnerabilities or outdated components. Regular audits help in maintaining the security integrity of blockchain networks and ensuring compliance with industry standards and regulations.

## Code for getnodeinfo


```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"
headers = {
    'accept': 'application/json',
    'content-type': 'application/json'
}

payload = {
  "address": "TZ4UXDV5ZhNW7fb2AMSbgfAEZ7hWsnYS2g",
  "visible": true
}

response = requests.post(url, headers=headers, data=json.dumps(data))
print(response.json())
```
## Common Errors

Common Errors  
When using the getnodeinfo HTTP REST API Tron method, the following issues may occur:  
- **Invalid API Endpoint**: If you receive a 404 error, ensure that the endpoint URL is correctly specified and matches the Tron node's address. Double-check the base URL and path for any typographical errors.  
- **Unauthorized Access**: A 401 error indicates that your request lacks valid authentication credentials. Verify that you have included the correct API key or token in your request headers.  
- **Malformed JSON Request**: If you encounter a 400 error, your request payload may be improperly formatted. Check that your JSON structure adheres to the expected schema and all required fields are correctly included.  
- **Node Synchronization Delay**: In cases where the node information appears outdated, the node might be synchronizing. Ensure that your node is fully synced with the Tron network to retrieve the most current data.  

Using the getnodeinfo method in Web3 applications provides critical insights into the status and configuration of Tron nodes. This information allows developers to monitor node health, optimize network interactions, and ensure robust connectivity within decentralized applications. By leveraging this method, developers can enhance the reliability and performance of their Web3 solutions.

### conclusion

The provided JSON snippet appears to be a part of the response from the getnodeinfo HTTP API in the Tron network, which offers crucial details about the node's status and configuration. By utilizing the getnodeinfo API, developers can effectively monitor and manage Tron nodes, ensuring optimal performance and connectivity in the blockchain ecosystem.
