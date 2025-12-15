---
description: >-
  Example code for the eth_subscribe JSON RPC method. Сomplete guide on how to
  use eth_subscribe JSON RPC in GetBlock Web3 documentation.
---

# eth\_subscribe - Arbitrum

This method allows developers to receive real-time notifications regarding new block headers on the Arbitrum blockchain; it sends notifications whenever a new block is added.

{% hint style="info" %}
This method is only available through a WebSocket endpoint.
{% endhint %}

#### Parameters

| Field             | Type   | Required | Description                                                                                        |
| ----------------- | ------ | -------- | -------------------------------------------------------------------------------------------------- |
| subscription name | string | Yes      | The type of event to subscribe to (e.g., `newHeads`, `logs,` `newPendingTransactions,` `syncing`). |

#### Request

{% hint style="success" %}
Note that subscriptions require a WebSocket connection and [WebSocket cat](https://www.npmjs.com/package/wscat) for you to use this method in the console.Install WebSocket cat with:`npm install -g wscat`
{% endhint %}

{% tabs %}
{% tab title="WSCAT" %}
```bash
$ wscat -c wss://go.getblock.us/<ACCESS_TOKEN>
# Wait for the connection to be established

Connected (press CTRL+C to quit)

> {"id":1,"jsonrpc":"2.0","method":"eth_subscribe","params":["newHeads"]}
```
{% endtab %}

{% tab title="Javascript" %}
```javascript
import WebSocket from ('ws');

const webSocket = new WebSocket('wss://go.getblock.us/<ACCESS_TOKEN>');

async function subscribeToNewBlocks() {

  const request = {
    id: 1,
    jsonrpc: '2.0',
    method: 'eth_subscribe',
    params: ['newHeads'],
  };

  const onOpen = (event) => {
    webSocket.send(JSON.stringify(request));
  };

  const onMessage = (event) => {
    const response = JSON.parse(event.data);
    console.log(response);
  };

  try {
    webSocket.addEventListener('open', onOpen);
    webSocket.addEventListener('message', onMessage);
  } catch (error) {
    console.error(error);
  }
}

subscribeToNewBlocks();
```
{% endtab %}
{% endtabs %}

#### Response

```java
{
  "jsonrpc": "2.0",
  "id": "1",
  "result": "0x9c7c3f3c3c63b1b89ac0c1e0b0d3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8090a1b"
}
```

#### Reponse Parameter Definition

| Field Name                  | Data Type      | Description                                                                                                                                                                                               |
| --------------------------- | -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **subscription**            | string         | The subscription ID                                                                                                                                                                                       |
| **object**                  | object         | A block object containing the following fields:                                                                                                                                                           |
| **object.number**           | string or null | The block number of the requested block, encoded as hexadecimal. `null` if the block is pending                                                                                                           |
| **object.hash**             | string or null | The block hash of the requested block. `null` if the block is pending                                                                                                                                     |
| **object.parenthash**       | string         | Hash of the previous block used to generate the current block. Also known as the 'parent block'                                                                                                           |
| **object.nonce**            | string or null | The hash used to demonstrate proof-of-work. `null` if the block is pending. Returns `0x0000000000000000` when the consensus is proof-of-stake                                                             |
| **object.sha3uncles**       | string         | The hash of the list of uncles included in the block. Used to identify the block uniquely and to verify the integrity of the block's data                                                                 |
| **object.logsbloom**        | string or null | The bloom filter for the logs of the block, a data structure that allows for efficient membership testing of elements in a set (the logs included in the block). `null` if pending                        |
| **object.transactionsroot** | string         | The root of the transaction trie of the block. Allows Arbitrum nodes to verify the integrity of the transactions in a block                                                                               |
| **object.stateroot**        | string         | The root of the final state trie of the block. Included in the block header and used to verify the integrity of the state at the time the block was processed                                             |
| **object.receiptsroot**     | string         | The root of the receipts trie of the block. A 32-byte hash of the root node of the receipts trie of all transactions in the block. Used to verify the integrity of the receipts data for all transactions |
| **object.miner**            | string         | The address of the miner receiving the reward                                                                                                                                                             |
| **object.difficulty**       | string         | A measure of how hard it is to find a valid block for the Arbitrum blockchain. A number that increases as more miners join the network and more blocks are added to the chain, encoded as hexadecimal     |
| **object.totaldifficulty**  | string         | The cumulative sum of the difficulty of all blocks that have been mined in the Arbitrum network since the inception of the network. Measures the overall security and integrity of the Arbitrum network   |
| **object.extradata**        | string         | Extra data included in a block by the miner who mined it. Often includes messages or other information related to the block                                                                               |
| **object.size**             | string         | The size of this block in bytes as an integer value, encoded as hexadecimal                                                                                                                               |
| **object.gaslimit**         | string         | The maximum gas allowed in this block, encoded as hexadecimal                                                                                                                                             |
| **object.gasused**          | string         | The total used gas by all transactions in this block, encoded as hexadecimal                                                                                                                              |
| **object.timestamp**        | string         | The Unix timestamp for when the block was collated                                                                                                                                                        |
| **object.transactions**     | array          | An array of transaction objects. See eth\_getTransactionByHash for the exact shape                                                                                                                        |
| **object.uncles**           | array          | An array of uncle hashes                                                                                                                                                                                  |

#### Use case

The `eth_subscribe` method helps developers to:

* Build real-time dashboards that stream block and transaction activity.
* Monitor contract events instantly without polling.
* Detect pending transactions for mempool analytics, wallets, and trading bots.
* Track node sync progress for DevOps use cases.
* Receive event-based updates for DeFi protocols, NFT marketplaces, or governance systems

#### Error handling

| Status Code | Error Message              | Cause                             |
| ----------- | -------------------------- | --------------------------------- |
| 403         | Forbidden                  | Missing or invalid ACCESS\_TOKEN. |
| -32601      | notification not supported |                                   |

#### Integration with Web3

The `eth_subscribe` method helps developers to:

* Build real-time dashboards that stream block and transaction activity.
* Monitor contract events instantly without polling.
* Detect pending transactions for mempool analytics, wallets, and trading bots.
* Track node sync progress for DevOps use cases.
* Receive event-based updates for DeFi protocols, NFT marketplaces, or governance systems.
