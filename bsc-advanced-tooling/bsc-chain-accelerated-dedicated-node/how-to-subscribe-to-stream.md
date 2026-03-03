---
description: >-
  Learn how to direct real-time access to transaction and block streams sourced
  from the BDN, bypassing RPC node-level execution and processing entirely.
---

# How to Subscribe to Stream

Stream subscription provides you with direct access to blockchain data as it propagates through the BDN network. This is very good for users e.g traders who want to get data earlier e.g new released token \
\
With a stream subscription, you enjoy:

* **Faster blocks**: Receive new blocks before standard RPC propagation
* **Mempool access**: See pending transactions before they're mined
* **Lower latency:** Edge infrastructure minimizes network hops

### Quickstart

Here, you will be checking if you are connected to streams

{% tabs %}
{% tab title="Endpoint" %}
```bash
wss://go.getblock.asia/<ACCESS_TOKEN>
```
{% endtab %}

{% tab title="Code" %}
{% code title="index.js" %}
```javascript
//npm install ws dotenv
import WebSocket from "ws";
import "dotenv/config";

const ws = new WebSocket(
  `wss://go.getblock.io/${process.env.ACCESS_TOKEN}`,
);

ws.on("open", () => {
  console.log("Connected to BSC streams");
});

ws.on("error", (err) => {
  console.error("WebSocket error:", err.message);
});
```
{% endcode %}
{% endtab %}

{% tab title="Response" %}
```bash
> bsc-node@1.0.0 start
> node index.js

Connected to BSC streams
```
{% endtab %}
{% endtabs %}

### How to Subscribe to Streams

To subscribe to streams, there are four parameters you can make use of as seen below:&#x20;

| Stream       | Description                              | Use Case                    |
| ------------ | ---------------------------------------- | --------------------------- |
| `bdnBlocks`  | New blocks with headers and transactions | Block monitoring, analytics |
| `newTxs`     | New transactions entering mempool        | MEV detection, copy trading |
| `pendingTxs` | Pending transactions                     | Front-running protection    |
| `txReceipts` | Transaction receipts after confirmation  | Trade confirmation          |

#### 1. Block Streams (bdnBlocks)

Subscribe to new blocks as they're produced.

**Include Options**

| Field          | Description                                               |
| -------------- | --------------------------------------------------------- |
| `header`       | Block header (number, hash, timestamp, gasUsed, gasLimit) |
| `transactions` | Full transaction list                                     |
| `hash`         | Block hash only                                           |

**Example: Subscribe to Blocks**

{% tabs %}
{% tab title="Code" %}
{% code title="subscribe-blocks.js" %}
```javascript
import WebSocket from "ws";
import "dotenv/config";

const ws = new WebSocket(`wss://go.getblock.io/${process.env.ACCESS_TOKEN}`);

ws.on("open", () => {
  console.log("Connected");

  // Subscribe to new blocks
  ws.send(
    JSON.stringify({
      jsonrpc: "2.0",
      id: 1,
      method: "subscribe",
      params: ["bdnBlocks", { include: ["header", "transactions", "hash"] }],
    }),
  );
});

ws.on("message", (data) => {
  const message = JSON.parse(data);

  // Subscription confirmation
  if (message.id === 1 && message.result) {
    console.log("Subscribed! ID:", parseInt(message.result));
    return;
  }

  // Block notification
  if (message.params?.result) {
    const block = message.params.result;
    console.log("New block:", {
      number: parseInt(block.header.number, 16),
      hash: block.header.hash,
      txCount: block.transactions?.length || 0,
    });
  }
});

```
{% endcode %}
{% endtab %}

{% tab title="Response" %}
```json
Connected
Subscribed! ID: 9
New block: { number: 84411561, hash: undefined, txCount: 86 }
New block: { number: 84411562, hash: undefined, txCount: 105 }
New block: { number: 84411563, hash: undefined, txCount: 66 }
New block: { number: 84411564, hash: undefined, txCount: 66 }
New block: { number: 84411565, hash: undefined, txCount: 78 }
New block: { number: 84411566, hash: undefined, txCount: 93 }
New block: { number: 84411567, hash: undefined, txCount: 63 }
```
{% endtab %}
{% endtabs %}

#### 2. Transaction Streams (newTxs / pendingTxs)

Subscribe to mempool transactions as they propagate through the network.

**Include Options**

| Field       | Description             |
| ----------- | ----------------------- |
| `tx_hash`   | Transaction hash        |
| `from`      | Sender address          |
| `to`        | Recipient address       |
| `value`     | Transaction value (hex) |
| `gas_price` | Gas price (hex)         |
| `input`     | Transaction calldata    |

**Filters**

Reduce bandwidth by filtering server-side:

| Filter      | Type  | Description                                    |
| ----------- | ----- | ---------------------------------------------- |
| `to`        | array | Transaction recipient addresses                |
| `from`      | array | Transaction sender addresses                   |
| `method_id` | array | Function selectors (first 4 bytes of calldata) |

**Example: Monitor PancakeSwap Transactions**

{% tabs %}
{% tab title="pendingTxs code" %}
<pre class="language-javascript" data-title="monitor-pancakeswap.js"><code class="lang-javascript">import WebSocket from "ws";
import { ethers } from "ethers";
import "dotenv/config";

const PANCAKE_ROUTER = "0x10ED43C718714eb63d5aA57B78B54704E256024E";

const SWAP_METHODS = {
  "0x38ed1739": "swapExactTokensForTokens",
  "0x7ff36ab5": "swapExactETHForTokens",
  "0x18cbafe5": "swapExactTokensForETH",
  "0xfb3bdb41": "swapETHForExactTokens",
  "0x5c11d795": "swapExactTokensForTokensSupportingFeeOnTransferTokens",
  "0xb6f9de95": "swapExactETHForTokensSupportingFeeOnTransferTokens",
};

function monitorPancakeSwap() {
  const ws = new WebSocket(`wss://go.getblock.io/${process.env.ACCESS_TOKEN}`);

  ws.on("open", () => {
    console.log("🔌 Connected to BSC stream");
    console.log("📊 Monitoring PancakeSwap...\n");

<strong>    ws.send(
</strong><strong>      JSON.stringify({
</strong><strong>        jsonrpc: "2.0",
</strong><strong>        id: 1,
</strong><strong>        method: "subscribe",
</strong><strong>        params: [
</strong><strong>          "pendingTxs",
</strong><strong>          {
</strong><strong>            include: ["tx_hash", "from", "to", "value", "input", "gas_price"],
</strong><strong>            filters: {
</strong><strong>              to: [PANCAKE_ROUTER],
</strong><strong>            },
</strong><strong>          },
</strong><strong>        ],
</strong><strong>      }),
</strong><strong>    );
</strong><strong>  });
</strong>
  ws.on("message", (data) => {
    const msg = JSON.parse(data);

    // Subscription confirmed
    if (msg.id === 1) {
      console.log("✅ Subscribed to PancakeSwap transactions\n");
      return;
    }

    // New transaction
    if (msg.params?.result) {
      const tx = msg.params.result;
      const methodId = tx.input?.slice(0, 10);
      const methodName = SWAP_METHODS[methodId] || "unknown";

      if (SWAP_METHODS[methodId]) {
        const value = ethers.formatEther(tx.value || "0");
        const gasPrice = ethers.formatUnits(tx.gas_price || "0", "gwei");

        console.log("🔄 Swap detected:");
        console.log(`   Hash: ${tx.tx_hash.slice(0, 18)}...`);
        console.log(`   Method: ${methodName}`);
        console.log(`   From: ${tx.from.slice(0, 10)}...`);
        console.log(`   Value: ${value} BNB`);
        console.log(`   Gas: ${gasPrice} gwei`);
        console.log("");
      }
    }
  });

  ws.on("close", () => {
    console.log("Disconnected, reconnecting in 5s...");
    setTimeout(monitorPancakeSwap, 5000);
  });

  ws.on("error", (err) => {
    console.error("WebSocket error:", err.message);
  });
}

monitorPancakeSwap();

</code></pre>
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

{% tab title="newTx code" %}
<pre class="language-js" data-overflow="wrap"><code class="lang-js">import WebSocket from 'ws';
import 'dotenv/config';

const PANCAKE_ROUTER = '0x10ED43C718714eb63d5aA57B78B54704E256024E';

const SWAP_METHODS = {
  '0x38ed1739': 'swapExactTokensForTokens',
  '0x7ff36ab5': 'swapExactETHForTokens',
  '0x18cbafe5': 'swapExactTokensForETH',
  '0xfb3bdb41': 'swapETHForExactTokens',
};

function monitorNewTxs() {
  const ws = new WebSocket(`wss://go.getblock.io/${process.env.ACCESS_TOKEN}`);

  ws.on('open', () => {
    console.log('Connected to BSC stream');
    console.log('Monitoring confirmed PancakeSwap swaps...\n');

<strong>    ws.send(JSON.stringify({
</strong><strong>      jsonrpc: '2.0',
</strong><strong>      id: 1,
</strong><strong>      method: 'subscribe',
</strong><strong>      params: ['newTxs', {
</strong><strong>        include: ['tx_hash', 'from', 'to', 'value', 'input'],
</strong><strong>        filters: {
</strong><strong>          to: [PANCAKE_ROUTER],
</strong><strong>          method_id: Object.keys(SWAP_METHODS)
</strong><strong>        }
</strong><strong>      }]
</strong><strong>    }));
</strong><strong>  });
</strong>
  ws.on('message', (data) => {
    const msg = JSON.parse(data);

    if (msg.id === 1) {
      console.log('Subscribed to confirmed PancakeSwap transactions\n');
      return;
    }

    if (msg.params?.result) {
      const tx = msg.params.result;
      const methodId = tx.input?.slice(0, 10);
      const methodName = SWAP_METHODS[methodId] || 'unknown';

      console.log('Confirmed swap:');
      console.log(`  Hash:   ${tx.tx_hash}`);
      console.log(`  Method: ${methodName}`);
      console.log(`  From:   ${tx.from}`);
      console.log(`  To:     ${tx.to}`);
      console.log(`  Value:  ${tx.value}`);
      console.log('');
    }
  });

  ws.on('close', () => {
    console.log('Disconnected, reconnecting in 5s...');
    setTimeout(monitorNewTxs, 5000);
  });

  ws.on('error', (err) => {
    console.error('WebSocket error:', err.message);
  });
}

monitorNewTxs();

</code></pre>
{% endtab %}

{% tab title="Response" %}
{% code overflow="wrap" %}
```bash
{
    "jsonrpc":"2.0",
    "id":1,
    "result":"0x11"
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

#### 3. Transaction Receipts (txReceipts)

Subscribe to transaction confirmations.

**Include Options**

| Field     | Description              |
| --------- | ------------------------ |
| `receipt` | Full transaction receipt |
| `logs`    | Event logs emitted       |

Example:&#x20;

{% tabs %}
{% tab title="Code" %}
<pre class="language-javascript"><code class="lang-javascript">import WebSocket from 'ws';
import 'dotenv/config';

const ws = new WebSocket(`wss://go.getblock.io/${process.env.ACCESS_TOKEN}`);

ws.on("open", () => {
  console.log("Connected");

  // Subscribe to new blocks
<strong>  ws.send(JSON.stringify({
</strong><strong>    jsonrpc: '2.0',
</strong><strong>    id: 1,
</strong><strong>    method: 'subscribe',
</strong><strong>    params: ['txReceipts', {
</strong><strong>      include: ['receipt', 'logs']
</strong><strong>    }]
</strong><strong>  }));
</strong><strong>});
</strong>
ws.on("message", (data) => {
  const message = JSON.parse(data);
  console.log(message)})
</code></pre>
{% endtab %}

{% tab title="Response" %}
{% code overflow="wrap" %}
```bash
{ 
  jsonrpc: '2.0', 
  id: 1, 
  result: '0x10' 
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Unsubscribing

To stop receiving notifications, unsubscribe using the subscription ID:

{% tabs %}
{% tab title="code sample" %}
{% code title="unsubscribe.js" overflow="wrap" %}
```js
import WebSocket from "ws";
import "dotenv/config";
const ws = new WebSocket(`wss://go.getblock.io/${process.env.ACCESS_TOKEN}`);
ws.on("open", () => {
  console.log("Connected");

ws.send(
  JSON.stringify({
    jsonrpc: "2.0",
    id: 10,
    method: "unsubscribe",
    params: ["23"], // Subscription ID
  }),
);})

ws.on("message", (data) => {
  const message = JSON.parse(data);
  console.log(JSON.stringify(message, null, 2));
});
```
{% endcode %}
{% endtab %}

{% tab title="Response" %}
{% code overflow="wrap" %}
```bash
{ 
    jsonrpc: '2.0', 
    id: 10, 
    result: true 
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

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

### Next Step

1. [Submitting transactions to public mempool ](how-to-submit-transactions-to-public-mempool.md)with accelerated propagation
2. Using private transactions for [MEV protection ](../bsc-accelerated-dedicated-node/sending-transactions-to-private-mempool-priority-fee/)or [without MEV Protection](../bsc-accelerated-dedicated-node/how-to-submit-transaction-to-private-mempool.md)
