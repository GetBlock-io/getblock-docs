---
description: >-
  Example code for the eth_blockNumber JSON-RPC method. Complete guide on how to
  use eth_blockNumber JSON-RPC in GetBlock Web3 documentation.
---

# eth\_blockNumber - Monad

This method returns the number of the most recent block.

{% hint style="warning" %}
Due to Monad's \~400ms block time, block numbers increase much faster than on Ethereum (\~12 second blocks). Applications should account for this when implementing polling or confirmation tracking.
{% endhint %}

### Parameters

* None

### Request

{% include "../../.gitbook/includes/curl-location-request-p....md" %}

### Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x1b4"
}
```

### Response Parameters

| Field  | Type   | Description                                     |
| ------ | ------ | ----------------------------------------------- |
| result | string | The current block number in hexadecimal format. |

### Use Case

The `eth_blockNumber` method is essential for:

* Monitoring blockchain height in real-time
* Synchronization status checks
* Building block explorers and dashboards
* Tracking transaction confirmations
* Implementing polling for new blocks
* High-frequency trading applications on Monad's fast block times

### Error Handling

| Status Code | Error Message | Cause                            |
| ----------- | ------------- | -------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS-TOKEN. |

### Web3 Integration

{% include "../../.gitbook/includes/import-ethers-from-eth....md" %}

