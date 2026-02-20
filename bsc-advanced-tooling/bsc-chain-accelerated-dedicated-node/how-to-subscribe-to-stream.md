---
description: >-
  Learn how to direct real-time access to transaction and block streams sourced
  from the BDN, bypassing RPC node-level execution and processing entirely.
---

# How to Subscribe to Stream

Stream subscription provides you with direct access to blockchain data as it propagates through the BDN network. Unlike standard RPC subscriptions that must wait for node-level processing, streams deliver data directly from the high-speed distribution layer.

## Why Use Streams

Streams bypass the node's internal processing pipeline:

<figure><img src="../../.gitbook/assets/image (2).png" alt="Stream subscription pipeline"><figcaption></figcaption></figure>

This architecture saves up to 10 milliseconds compared to node-based subscriptions—critical for latency-sensitive systems. \
\
With stream subscription, you enjoy:

* **Faster blocks** — Receive new blocks before standard RPC propagation
* **Mempool access** — See pending transactions before they're mined
* **Lower latency** — Edge infrastructure minimizes network hops

## Performance Characteristics

| Metric             | Value                |
| ------------------ | -------------------- |
| Block latency      | <50ms from validator |
| Message throughput | 70,000+ msg/sec      |
| Reconnect time     | \~3 seconds          |

### Latency Comparison

| Source               | Typical Latency  |
| -------------------- | ---------------- |
| Standard RPC         | 200-500ms        |
| GetBlock MEV Streams | <50ms            |
| **Improvement**      | **4-10x faster** |

### Available Streams Methods

| Stream       | Description                              | Use Case                    |
| ------------ | ---------------------------------------- | --------------------------- |
| `bdnBlocks`  | New blocks with headers and transactions | Block monitoring, analytics |
| `newTxs`     | New transactions entering mempool        | MEV detection, copy trading |
| `pendingTxs` | Pending transactions                     | Front-running protection    |
| `txReceipts` | Transaction receipts after confirmation  | Trade confirmation          |

## Quickstart

Here you will be checking if you are connected to streams

{% tabs %}
{% tab title="Endpoint" %}
```bash
wss://bsc.getblock.io/mev/ws?api_key=YOUR_API_KEY
```
{% endtab %}

{% tab title="Code" %}
{% code title="connection.js" %}
```javascript
//npm install ws
const WebSocket = require('ws');
const API_KEY = 'YOUR_API_KEY';
const ws = new WebSocket(`wss://bsc.getblock.io/mev/ws?api_key=${API_KEY}`);

ws.on('open', () => {
  console.log('Connected to BSC streams');
});

ws.on('error', (err) => {
  console.error('WebSocket error:', err.message);
});
```
{% endcode %}
{% endtab %}

{% tab title="Response" %}
```
```
{% endtab %}
{% endtabs %}

## Subscribing to Streams

{% tabs %}
{% tab title="Subscribe Request Format" %}
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "subscribe",
  "params": ["<stream_name>", { "include": [...], "filters": {...} }]
}
```
{% endtab %}

{% tab title="Subscribe Response" %}
When your subscription is accepted, you receive a subscription ID:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "550e8400-e29b-41d4-a716-446655440000"
}
```

{% hint style="warning" %}
Save this ID to unsubscribe later.
{% endhint %}
{% endtab %}

{% tab title="Stream Notification Format" %}
After subscribing, you receive notifications in this format:

```json
{
  "jsonrpc": "2.0",
  "method": "subscribe",
  "params": {
    "subscription": "550e8400-e29b-41d4-a716-446655440000",
    "result": { /* stream data */ }
  }
}
```
{% endtab %}
{% endtabs %}

## 1. Block Streams (bdnBlocks)

Subscribe to new blocks as they're produced.

### Include Options

| Field          | Description                                               |
| -------------- | --------------------------------------------------------- |
| `header`       | Block header (number, hash, timestamp, gasUsed, gasLimit) |
| `transactions` | Full transaction list                                     |
| `hash`         | Block hash only                                           |

### Example: Subscribe to Blocks

{% tabs %}
{% tab title="Code" %}
{% code title="subscribe-blocks.js" %}
```javascript
const WebSocket = require('ws');

const API_KEY = 'YOUR_API_KEY';
const ws = new WebSocket(`wss://bsc.getblock.io/mev/ws?api_key=${API_KEY}`);

ws.on('open', () => {
  console.log('Connected');
  
  // Subscribe to new blocks
  ws.send(JSON.stringify({
    jsonrpc: '2.0',
    id: 1,
    method: 'subscribe',
    params: ['bdnBlocks', { include: ['header', 'transactions'] }]
  }));
});

ws.on('message', (data) => {
  const message = JSON.parse(data);
  
  // Subscription confirmation
  if (message.id === 1 && message.result) {
    console.log('Subscribed! ID:', message.result);
    return;
  }
  
  // Block notification
  if (message.params?.result) {
    const block = message.params.result;
    console.log('New block:', {
      number: parseInt(block.header.number, 16),
      hash: block.header.hash,
      txCount: block.transactions?.length || 0
    });
  }
});
```
{% endcode %}
{% endtab %}

{% tab title="Response" %}
```json
{
  "header": {
    "number": "0x2a43b7c",
    "hash": "0xabc123...",
    "timestamp": "0x65f1a2b3",
    "gasUsed": "0x1c9c380",
    "gasLimit": "0x8583b00"
  },
  "transactions": [...]
}
```
{% endtab %}
{% endtabs %}

## 2. Transaction Streams (newTxs / pendingTxs)

Subscribe to mempool transactions as they propagate through the network.

### Include Options

| Field       | Description             |
| ----------- | ----------------------- |
| `tx_hash`   | Transaction hash        |
| `from`      | Sender address          |
| `to`        | Recipient address       |
| `value`     | Transaction value (hex) |
| `gas_price` | Gas price (hex)         |
| `input`     | Transaction calldata    |

### Filters

Reduce bandwidth by filtering server-side:

| Filter      | Type  | Description                                    |
| ----------- | ----- | ---------------------------------------------- |
| `to`        | array | Transaction recipient addresses                |
| `from`      | array | Transaction sender addresses                   |
| `method_id` | array | Function selectors (first 4 bytes of calldata) |

### Example: Monitor PancakeSwap Transactions

{% tabs %}
{% tab title="Code" %}
{% code title="monitor-pancakeswap.js" %}
```javascript
const WebSocket = require('ws');
const { ethers } = require('ethers');

const API_KEY = 'YOUR_API_KEY';
const PANCAKE_ROUTER = '0x10ED43C718714eb63d5aA57B78B54704E256024E';

const SWAP_METHODS = {
  '0x38ed1739': 'swapExactTokensForTokens',
  '0x7ff36ab5': 'swapExactETHForTokens',
  '0x18cbafe5': 'swapExactTokensForETH',
  '0xfb3bdb41': 'swapETHForExactTokens',
  '0x5c11d795': 'swapExactTokensForTokensSupportingFeeOnTransferTokens',
  '0xb6f9de95': 'swapExactETHForTokensSupportingFeeOnTransferTokens'
};

function monitorPancakeSwap() {
  const ws = new WebSocket(`wss://bsc.getblock.io/mev/ws?api_key=${API_KEY}`);
  
  ws.on('open', () => {
    console.log('🔌 Connected to BSC stream');
    console.log('📊 Monitoring PancakeSwap...\n');
    
    ws.send(JSON.stringify({
      jsonrpc: '2.0',
      id: 1,
      method: 'subscribe',
      params: ['pendingTxs', {
        include: ['tx_hash', 'from', 'to', 'value', 'input', 'gas_price'],
        filters: {
          to: [PANCAKE_ROUTER]
        }
      }]
    }));
  });
  
  ws.on('message', (data) => {
    const msg = JSON.parse(data);
    
    // Subscription confirmed
    if (msg.id === 1) {
      console.log('✅ Subscribed to PancakeSwap transactions\n');
      return;
    }
    
    // New transaction
    if (msg.params?.result) {
      const tx = msg.params.result;
      const methodId = tx.input?.slice(0, 10);
      const methodName = SWAP_METHODS[methodId] || 'unknown';
      
      if (SWAP_METHODS[methodId]) {
        const value = ethers.formatEther(tx.value || '0');
        const gasPrice = ethers.formatUnits(tx.gas_price || '0', 'gwei');
        
        console.log('🔄 Swap detected:');
        console.log(`   Hash: ${tx.tx_hash.slice(0, 18)}...`);
        console.log(`   Method: ${methodName}`);
        console.log(`   From: ${tx.from.slice(0, 10)}...`);
        console.log(`   Value: ${value} BNB`);
        console.log(`   Gas: ${gasPrice} gwei`);
        console.log('');
      }
    }
  });
  
  ws.on('close', () => {
    console.log('Disconnected, reconnecting in 5s...');
    setTimeout(monitorPancakeSwap, 5000);
  });
  
  ws.on('error', (err) => {
    console.error('WebSocket error:', err.message);
  });
}

monitorPancakeSwap();
```
{% endcode %}
{% endtab %}

{% tab title="Response" %}
```json
{
  "tx_hash": "0xdef456...",
  "from": "0xabc...",
  "to": "0x10ED43C718714eb63d5aA57B78B54704E256024E",
  "value": "0x16345785d8a0000",
  "gas_price": "0xb2d05e00",
  "input": "0x38ed1739..."
}
```
{% endtab %}

{% tab title="Filtering by Multiple Method IDs" %}
{% code title="filter-multiple-methods.js" %}
```javascript
ws.send(JSON.stringify({
  jsonrpc: '2.0',
  id: 1,
  method: 'subscribe',
  params: ['newTxs', {
    include: ['tx_hash', 'from', 'to', 'value', 'input'],
    filters: {
      to: ['0x10ED43C718714eb63d5aA57B78B54704E256024E'],  // PancakeSwap Router
      method_id: [
        '0x38ed1739',  // swapExactTokensForTokens
        '0x7ff36ab5',  // swapExactETHForTokens
        '0x18cbafe5',  // swapExactTokensForETH
        '0xfb3bdb41'   // swapETHForExactTokens
      ]
    }
  }]
}));
```
{% endcode %}
{% endtab %}
{% endtabs %}

## 3. Transaction Receipts (txReceipts)

Subscribe to transaction confirmations.

### Include Options

| Field     | Description              |
| --------- | ------------------------ |
| `receipt` | Full transaction receipt |
| `logs`    | Event logs emitted       |

### Example

```javascript
ws.send(JSON.stringify({
  jsonrpc: '2.0',
  id: 1,
  method: 'subscribe',
  params: ['txReceipts', {
    include: ['receipt', 'logs']
  }]
}));
```

## Unsubscribing

To stop receiving notifications, unsubscribe using the subscription ID:

```javascript
ws.send(JSON.stringify({
  "jsonrpc": "2.0",
  "id": 10,
  "method": "unsubscribe",
  "params": ["550e8400-e29b-41d4-a716-446655440000"]  // Subscription ID
}));
```

## Best Practices

1\. Request Only What You Need

{% tabs %}
{% tab title="Don't ❌" %}
```js
// ❌ Bad: Request everything
include: ['tx_contents']  // Large payloads, high bandwidth
```
{% endtab %}

{% tab title="Do ✅" %}
```js
// ✅ Good: Request specific fields
include: ['tx_hash', 'from', 'to', 'value']  // Small payloads
```
{% endtab %}
{% endtabs %}

2. Use Server-Side Filters

{% tabs %}
{% tab title="Don't ❌" %}
```js
// ❌ Bad: Filter client-side
params: ['newTxs', { include: ['tx_contents'] }]
// Then filter in your code... wasteful!
```
{% endtab %}

{% tab title="Do ✅" %}
```js
// ✅ Good: Filter server-side
params: ['newTxs', {
  include: ['tx_hash', 'to', 'input'],
  filters: { to: ['0xTargetContract'] }
}]
```
{% endtab %}
{% endtabs %}

3. Handle Reconnection

```javascript
function createConnection() {
  const ws = new WebSocket(`wss://bsc.getblock.io/mev/ws?api_key=${API_KEY}`);
  
  ws.on('close', () => {
    console.log('Disconnected, reconnecting...');
    setTimeout(createConnection, 5000);
  });
  
  ws.on('open', () => {
    // Re-subscribe after reconnect
    subscribeToFeeds(ws);
  });
  
  return ws;
}
```

4. Process Messages Asynchronously

```javascript
ws.on('message', async (data) => {
  // Don't block the message loop
  setImmediate(() => {
    processMessage(JSON.parse(data));
  });
});
```

## Rate Limits

| Tier       | Concurrent Subscriptions | Message Rate |
| ---------- | ------------------------ | ------------ |
| Standard   | 5                        | 1,000/sec    |
| Pro        | 20                       | 10,000/sec   |
| Enterprise | Unlimited                | Unlimited    |

_**Contact**_ [_**support**_](mailto:support@getblock.io) _**to upgrade your tier.**_

## Troubleshooting

| Problem              | Solution                                                                                                                                                                                 |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| No messages received | <p></p><ul><li>Check subscription response—did you receive a subscription ID?</li><li>Verify filters aren't too restrictive</li><li>Confirm your API key is valid and active</li></ul>   |
| High latency         | <p></p><ul><li>Reduce the number of <code>include</code> fields</li><li>Add filters to reduce message volume</li><li>Check your network connection</li></ul>                             |
| Missed messages      | <p></p><ul><li>Process messages asynchronously</li><li>Increase your message queue buffer</li><li>Consider multiple connections for different streams</li></ul>                          |
| Connection Drop      | <p></p><ul><li>Implement automatic reconnection (see example above)</li><li>Re-subscribe after each reconnect—subscriptions don't persist</li><li>Check your network stability</li></ul> |
