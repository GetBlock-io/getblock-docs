---
description: >-
  A step by step guide on how to build an Hyperliquid Whale Tracker Bot using
  GetBlock API
icon: telegram
---

# How to Build a Hyperliquid Whale Tracker Bot with GetBlock

Whales are entities, such as DAOs or companies, that hold a large amount of a particular cryptocurrency, sufficient to influence the market price through their transactions. Their trade alone can either deflate or inflate the market price. While some whales engage in large trades that have a natural market effect, others may intentionally try to manipulate the market for profit. \
\
The Whale Tracking Bot is a bot that monitors on-chain activities of whales. It reports their transactions to you, thereby providing a clear analysis to inform predictions about potential market movements.

In this guide, you will learn:&#x20;

* Create a Telegram bot using [Botfather](https://telegram.me/BotFather)
* Get a GetBlock API access token
* Build the Hyperliquid whale tracker bot that:&#x20;
  * Tracks all trades on Hyperliquid as they happen
  * Identifies large trades based on configurable thresholds
  * Detects whale walls in the orderbook
  * Tracks and profiles recurring whale addresses
  * Sends an instant alert to your phone via Telegram
  * Utilizes smart batching to avoid Telegram API limits
  * Provides periodic summaries of whale activity

### Prerequisites

1. Basic knowledge of JavaScript
2. Must have installed [Node](https://nodejs.org/en/download/current) and [npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm/)
3. A [GetBlock account](https://account.getblock.io/)
4. A [Telegram account](how-to-build-a-hyperliquid-whale-tracker-bot-with-getblock.md#id-2.-configuration-settings)

### Step 1. Create Telegram Bot

1. **Bot Token**:
   * Open Telegram and search for `@BotFather`
   * Send `/newbot` and follow instructions
   * Copy the token provided and save it in `.env` file
2. **Chat ID**:
   * Search for `@raw_data_bot` in Telegram
   * Send `/start`&#x20;
   * Copy your user ID

### Step 2. Set up Development Environment

Before you begin, you need to set up your development environment:

```bash
mkdir hyperliquid-whale-alert-bot
cd hyperliquid-whale-alert-bot
npm init -y
npm i dotenv ws
npm i -D nodemon
```

### Step 3. Getting your HyperEVM Websocket Token

1. Log in to your [GetBlock account](https://account.getblock.io/)
2. On your dashboard, scroll and click on **Get Endpoint**
3. Select the **HyperEVM Mainnet** network
4. Under **API Interface,** select **WebSocket**
5. Click on **Create** to get your endpoint

{% hint style="warning" %}
Keep your endpoint safe, as it contains an access token
{% endhint %}

#### Project Structure

Create the following files to have a basic structure for your project:

```bash
├── index.js            // Main apllication
└── .env                // Environment variables
└── .gitignore          // Git ignore file
```

### Step 4. Set up Environment Variables

Update the `.env` file in the root of your project and add the following variables:

<pre class="language-bash"><code class="lang-bash"><strong># Telegram Configuration (Get from @BotFather and @userinfobot)
</strong>TELEGRAM_BOT_TOKEN=your-telegram-bot-token
TELEGRAM_CHAT_ID=your-ID

<strong># Whale Detection Thresholds
</strong>WHALE_THRESHOLD_USD=100000
WHALE_THRESHOLD_SIZE=50

<strong># Telegram Settings
</strong>TELEGRAM_BATCH_ALERTS=true
TELEGRAM_BATCH_INTERVAL=10000

<strong># Markets to Track (comma-separated)
</strong>TRACKED_SYMBOLS=BTC,ETH,SOL,ARB,AVAX,HYPE

<strong># Optional: HyperEVM WebSocket from GetBlock
</strong>HYPEREVM_WSS=wss://go.getblock.io/&#x3C;ACCESS_TOKEN>
</code></pre>

### Step 5. Build Whale Tracker Bot

#### 1. Import dependencies:

Import the dependencies and load the environment variables from `.env` file

```javascript
// dependencies
import WebSocket from 'ws';
import { config } from 'dotenv';

// Load environment variables from .env file
config();
```

#### 2. Set up configuration settings:

Set up configuration settings for your application, including environment variables and other constants.

```javascript
const CONFIG = {
  // HyperEVM WebSocket endpoint from GetBlock
  HYPEREVM_WSS: process.env.HYPEREVM_WSS,
  
  // Whale thresholds
  WHALE_THRESHOLD_USD: Number(process.env.WHALE_THRESHOLD_USD) || 100000,
  WHALE_THRESHOLD_SIZE: Number(process.env.WHALE_THRESHOLD_SIZE) || 50,
  
  // Notification settings
  TELEGRAM_BOT_TOKEN: process.env.TELEGRAM_BOT_TOKEN || '',
  TELEGRAM_CHAT_ID: process.env.TELEGRAM_CHAT_ID || '',
  TELEGRAM_BATCH_ALERTS: process.env.TELEGRAM_BATCH_ALERTS !== 'false',
  TELEGRAM_BATCH_INTERVAL: Number(process.env.TELEGRAM_BATCH_INTERVAL) || 60 * 10000,
  
  // Tracked markets
  TRACKED_SYMBOLS: (process.env.TRACKED_SYMBOLS || 'BTC,ETH,SOL,ARB,AVAX,HYPE').split(','),
};
```

#### 3. Create the Main Class with Private Fields:

```javascript
class HyperliquidWhaleTracker {
  // Private fields (only accessible within the class)
  #ws = null;                    // Hyperliquid WebSocket connection
  #wsHyperEVM = null;            // HyperEVM WebSocket connection
  #reconnectAttempts = 0;        // Track reconnection attempts
  #maxReconnectAttempts = 5;     // Maximum reconnection tries
  #priceCache = new Map();       // Store current prices
  #whaleAddresses = new Map();   // Track whale wallet addresses
  #tradeCount = 0;               // Total whale trades detected
  #telegramQueue = [];           // Queue for Telegram messages
  #isSendingTelegram = false;    // Flag for queue processing
  #lastTelegramSent = 0;         // Timestamp of last message
  #TELEGRAM_DELAY = 1500;        // Delay between messages (ms)
  #pendingAlerts = [];           // Batch alerts before sending
  #batchTimer = null;            // Timer for batching

  constructor() {
    this.HYPERLIQUID_WS_URL = 'wss://api.hyperliquid.xyz/ws';
  }
}
```

**Why private fields?**

* `#` prefix makes fields private (ES2022 feature)
* Prevents external code from modifying internal state
* Better encapsulation and code organization

#### 4. Connect to Hyperliquid WebSocket:

```javascript
async connectHyperliquid() {
  console.log('Connecting to Hyperliquid Info WebSocket...');
  
  this.#ws = new WebSocket(this.HYPERLIQUID_WS_URL);

  this.#ws.on('open', () => {
    console.log('✅ Connected to Hyperliquid Info API');
    this.#subscribeToTrades();
    this.#subscribeToOrderbook();
  });

  this.#ws.on('message', (data) => this.#handleHyperliquidMessage(data));
  this.#ws.on('error', (error) => console.error('Hyperliquid error:', error.message));
  this.#ws.on('close', () => this.#handleReconnect('hyperliquid'));
}
```

**Event handlers:**

* `open` - Triggered when the connection is established
* `message` - Receives data from the server
* `error` - Handles connection errors
* `close` - Handles disconnection

#### 5. Subscribe to Data Streams:

```javascript
#subscribeToTrades() {
  // Subscribe to price updates
  this.#ws.send(JSON.stringify({
    method: 'subscribe',
    subscription: { type: 'allMids' }
  }));
  
  // Subscribe to trades for each tracked symbol
  for (const symbol of CONFIG.TRACKED_SYMBOLS) {
    this.#ws.send(JSON.stringify({
      method: 'subscribe',
      subscription: { type: 'trades', coin: symbol }
    }));
  }

  console.log('📡 Subscribed to Hyperliquid trades');
}

#subscribeToOrderbook() {
  // Subscribe to orderbook depth for whale wall detection
  for (const symbol of CONFIG.TRACKED_SYMBOLS) {
    this.#ws.send(JSON.stringify({
      method: 'subscribe',
      subscription: { type: 'l2Book', coin: symbol }
    }));
  }

  console.log('📊 Subscribed to orderbook data');
}
```

**Subscription types:**

* `allMids` - Current market prices for all symbols
* `trades` - Real-time trade executions
* `l2Book` - Level 2 orderbook (buy/sell walls)

#### 6. Handle Incoming Messages:

```java
#handleHyperliquidMessage(data) {
  try {
    const message = JSON.parse(data.toString());
    
    // Route messages based on channel type
    switch (message.channel) {
      case 'trades':
        message.data?.forEach(trade => this.#analyzeTrade(trade));
        break;
      case 'allMids':
        this.#updatePrices(message.data);
        break;
      case 'l2Book':
        this.#analyzeOrderbook(message.data);
        break;
    }
  } catch (error) {
    console.error('Error parsing message:', error.message);
  }
}
```

**Message routing:**

* Parse JSON data from WebSocket
* Route to the appropriate handler based on `channel` type
* Graceful error handling with try-catch

#### 7. Update Price Cache

```javascript
#updatePrices(midsData) {
  if (!midsData?.mids) return;
  
  // Store current prices in a Map for quick lookup
  for (const [symbol, price] of Object.entries(midsData.mids)) {
    this.#priceCache.set(symbol, parseFloat(price));
  }
}
```

**Why cache prices?**

* Used for calculating USD value of trades
* Avoids repeated API calls
* Provides a fallback when price data is missing

#### 8. Analyze Trades for Whales

```javascript
#analyzeTrade(trade) {
  try {
    const { coin, side, px, sz, time, user, hash, tid } = trade;
    const price = parseFloat(px);
    const size = parseFloat(sz);
    const value = price * size;  // Calculate USD value

    // Check if trade meets whale threshold
    if (value < CONFIG.WHALE_THRESHOLD_USD && size < CONFIG.WHALE_THRESHOLD_SIZE) {
      return;  // Not a whale trade, skip
    }

    // Track the whale's address
    if (user) {
      this.#trackWhaleAddress(user, value, coin);
    }

    // Send notification
    this.#notifyWhaleTrade({
      coin,
      side,
      price,
      size,
      value,
      time: new Date(time).toLocaleString(),
      user: user || 'Unknown',
      hash: hash || null,
      tradeId: tid || null
    });
    
    this.#tradeCount++;
  } catch (error) {
    console.error('Error analyzing trade:', error.message);
  }
}
```

**Trade data structure:**

* `coin` - Market symbol (BTC, ETH, etc.)
* `side` - 'B' for buy, 'A' for ask/sell
* `px` - Price (as string)
* `sz` - Size/quantity (as string)
* `user` - Wallet address of trader
* `hash` - Transaction hash
* `tid` - Trade ID

#### 9. Track Whale Addresses

```javascript
#trackWhaleAddress(address, value, coin) {
  const whale = this.#whaleAddresses.get(address);
  
  if (whale) {
    // Existing whale - update stats
    whale.totalVolume += value;
    whale.tradeCount++;
    whale.lastSeen = new Date();
    whale.coins.add(coin);
  } else {
    // New whale - create profile
    this.#whaleAddresses.set(address, {
      address,
      totalVolume: value,
      tradeCount: 1,
      firstSeen: new Date(),
      lastSeen: new Date(),
      coins: new Set([coin])
    });
  }
}
```

**Whale profile includes:**

* Total trading volume (cumulative)
* Number of trades executed
* First and last seen timestamps
* Set of markets they trade in

#### 10. Analyze Orderbook for Whale Walls

```javascript
#analyzeOrderbook(bookData) {
  try {
    const { coin, levels } = bookData;
    
    if (!Array.isArray(levels)) return;

    const largeOrders = [];

    // Analyze both bid (buy) and ask (sell) sides
    for (const [index, side] of ['BID', 'ASK'].entries()) {
      if (!Array.isArray(levels[index])) continue;

      for (const level of levels[index]) {
        // Handle different data formats
        const [price, size] = Array.isArray(level) 
          ? level 
          : [level.px, level.sz];

        if (!price || !size) continue;

        const value = parseFloat(price) * parseFloat(size);
        
        // Check if it's a whale-sized order
        if (value >= CONFIG.WHALE_THRESHOLD_USD) {
          largeOrders.push({ 
            side, 
            price: parseFloat(price), 
            size: parseFloat(size), 
            value 
          });
        }
      }
    }

    if (largeOrders.length > 0) {
      this.#notifyWhaleOrder(coin, largeOrders);
    }
  } catch (error) {
    console.error('Error analyzing orderbook:', error.message);
  }
}
```

**Orderbook structure:**

* `levels[0]` - Bid side (buy orders)
* `levels[1]` - Ask side (sell orders)
* Each level is `[price, size]` array

**Whale walls:**

* Large limit orders in the orderbook
* Can act as support (bids) or resistance (asks)
* Indicate where whales are defending price levels

#### 11. Send Trade Notifications

```javascript
#notifyWhaleTrade(trade) {
  const emoji = trade.side === 'B' ? '🟢' : '🔴';
  const sideText = trade.side === 'B' ? 'BUY' : 'SELL';
  const shortAddress = trade.user !== 'Unknown' 
    ? `${trade.user.slice(0, 6)}...${trade.user.slice(-4)}`
    : 'Unknown';
  
  // Get whale statistics if available
  let whaleStats = '';
  const whale = this.#whaleAddresses.get(trade.user);
  if (whale) {
    whaleStats = `\n📈 Whale Stats: ${whale.tradeCount} trades | $${whale.totalVolume.toLocaleString()} total volume`;
  }
  
  const consoleMessage = `
🐋 WHALE TRADE ALERT! 🐋

${emoji} ${sideText} ${trade.coin}

💰 Size: ${trade.size.toFixed(2)} contracts
💵 Price: $${trade.price.toFixed(2)}
📊 Value: $${trade.value.toLocaleString()}
👤 Trader: ${shortAddress}
${trade.user !== 'Unknown' ? `🔗 Address: ${trade.user}` : ''}${whaleStats}
${trade.hash ? `📝 TX Hash: ${trade.hash}` : ''}
${trade.tradeId ? `🆔 Trade ID: ${trade.tradeId}` : ''}
🕐 Time: ${trade.time}

${trade.value >= 500000 ? '🚨 MEGA WHALE! 🚨' : ''}
  `;

  console.log(consoleMessage);
  
  // Send to Telegram if configured
  if (CONFIG.TELEGRAM_BOT_TOKEN && CONFIG.TELEGRAM_CHAT_ID) {
    if (CONFIG.TELEGRAM_BATCH_ALERTS) {
      this.#addToBatch({ type: 'trade', emoji, sideText, ...trade });
    } else {
      this.#sendTelegramMessage(consoleMessage);
    }
  }
}
```

**Notification features:**

* Color-coded emojis (green=buy, red=sell)
* Shortened wallet address for readability
* Historical stats for known whales
* Special highlight for mega trades ($500k+)
* Optional Telegram integration

#### 12. Implement Telegram Batching

```javascript
#addToBatch(alert) {
  this.#pendingAlerts.push(alert);
  
  // Start batch timer if not already running
  if (!this.#batchTimer) {
    this.#batchTimer = setTimeout(() => {
      this.#sendBatchedAlerts();
    }, CONFIG.TELEGRAM_BATCH_INTERVAL);
  }
}

#sendBatchedAlerts() {
  console.log(`📦 Sending batched alerts: ${this.#pendingAlerts.length} trades`);
  
  if (this.#pendingAlerts.length === 0) {
    this.#batchTimer = null;
    return;
  }

  const totalValue = this.#pendingAlerts.reduce((sum, alert) => sum + alert.value, 0);
  const trades = this.#pendingAlerts.length;
  
  // Build compact batch message
  const alertsList = this.#pendingAlerts
    .map((alert, index) => {
      const shortAddr = alert.user !== 'Unknown' 
        ? `${alert.user.slice(0, 6)}...${alert.user.slice(-4)}`
        : 'Unknown';
      
      let line = `${index + 1}. ${alert.emoji} ${alert.sideText} ${alert.coin}\n`;
      line += `   💰 $${alert.value.toLocaleString()} | 👤 ${shortAddr}\n`;
      if (alert.value >= 500000) line += `   🚨 MEGA WHALE!\n`;
      return line;
    })
    .join('\n');
  
  const message = `🐋 WHALE ACTIVITY BATCH (${trades} trades, $${totalValue.toLocaleString()} total) 🐋\n\n${alertsList}\n🕐 ${new Date().toLocaleString()}`;
  
  console.log('📤 Adding batch message to Telegram queue...');
  this.#sendTelegramMessage(message);
  this.#pendingAlerts = [];
  this.#batchTimer = null;
}
```

**Why batch alerts?**

* Telegram has rate limits (1 message/second)
* Batching prevents hitting limits during high activity
* Combines multiple alerts into one clean summary
* Configurable interval (default: 10 seconds)

#### 13. Implement Telegram Queue System

```javascript
async #sendTelegramMessage(message) {
  if (!CONFIG.TELEGRAM_BOT_TOKEN || !CONFIG.TELEGRAM_CHAT_ID) return;

  // Add to queue instead of sending immediately
  this.#telegramQueue.push(message);
  
  // Start processing if not already running
  if (!this.#isSendingTelegram) {
    this.#processTelegramQueue();
  }
}

async #processTelegramQueue() {
  if (this.#telegramQueue.length === 0) {
    this.#isSendingTelegram = false;
    return;
  }

  this.#isSendingTelegram = true;
  const message = this.#telegramQueue.shift();

  try {
    // Respect rate limit (1.5 seconds between messages)
    const timeSinceLastSend = Date.now() - this.#lastTelegramSent;
    if (timeSinceLastSend < this.#TELEGRAM_DELAY) {
      await new Promise(resolve => 
        setTimeout(resolve, this.#TELEGRAM_DELAY - timeSinceLastSend)
      );
    }

    // Send to Telegram API
    const url = `https://api.telegram.org/bot${CONFIG.TELEGRAM_BOT_TOKEN}/sendMessage`;
    
    const response = await fetch(url, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        chat_id: CONFIG.TELEGRAM_CHAT_ID,
        text: message,
        parse_mode: 'HTML'
      })
    });

    this.#lastTelegramSent = Date.now();

    if (!response.ok) {
      const errorData = await response.json();
      console.error('❌ Telegram API error:', response.status, errorData.description || '');
      
      // Handle rate limiting
      if (response.status === 429) {
        const retryAfter = errorData.parameters?.retry_after || 5;
        console.log(`⏳ Rate limited. Waiting ${retryAfter}s...`);
        await new Promise(resolve => setTimeout(resolve, retryAfter * 1000));
        this.#telegramQueue.unshift(message);  // Re-add to front
      }
    } else {
      console.log('✅ Telegram message sent');
    }
  } catch (error) {
    console.error('❌ Telegram error:', error.message);
  }

  // Process next message in queue
  setTimeout(() => this.#processTelegramQueue(), this.#TELEGRAM_DELAY);
}
```

**Queue system benefits:**

* Messages are queued and processed sequentially
* Respects Telegram's rate limits
* Automatic retry on 429 errors
* Non-blocking (uses async/await)

#### 14. Test Telegram Connection

```javascript
async #testTelegramConnection() {
  try {
    const url = `https://api.telegram.org/bot${CONFIG.TELEGRAM_BOT_TOKEN}/getMe`;
    const response = await fetch(url);
    const data = await response.json();
    
    if (data.ok) {
      console.log(`✅ Telegram bot connected: @${data.result.username}`);
      await this.#sendTelegramMessage('🤖 Whale Tracker Started!\n\nMonitoring Hyperliquid for whale activity...');
      console.log('✅ Test message sent\n');
    } else {
      console.error('❌ Telegram error:', data.description);
    }
  } catch (error) {
    console.error('❌ Failed to connect to Telegram:', error.message);
    console.log('💡 Check your TELEGRAM_BOT_TOKEN\n');
  }
}
```

**Connection test:**

* Validates bot token on startup
* Shows bot username
* Sends test message
* Provides helpful error messages

#### 15. Implement Auto-Reconnection

```javascript
#handleReconnect(connection) {
  if (this.#reconnectAttempts >= this.#maxReconnectAttempts) {
    console.error(`Max reconnection attempts reached for ${connection}`);
    return;
  }

  this.#reconnectAttempts++;
  
  // Exponential backoff: 2s, 4s, 8s, 16s, 30s (max)
  const delay = Math.min(1000 * 2 ** this.#reconnectAttempts, 30000);
  
  console.log(`Reconnecting ${connection} in ${delay/1000}s... (Attempt ${this.#reconnectAttempts})`);
  
  setTimeout(() => {
    connection === 'hyperevm' ? this.connectHyperEVM() : this.connectHyperliquid();
  }, delay);
}
```

**Reconnection strategy:**

* Exponential backoff (delays increase)
* Maximum 5 attempts
* Caps at 30 seconds between tries
* Separate handling for different connections

#### 16. Generate Statistics Dashboard

```javascript
#printWhaleStats() {
  console.log('\n' + '='.repeat(60));
  console.log('📊 WHALE STATISTICS SUMMARY');
  console.log('='.repeat(60));
  console.log(`Total Whale Trades: ${this.#tradeCount}`);
  console.log(`Unique Whales: ${this.#whaleAddresses.size}\n`);

  if (this.#whaleAddresses.size === 0) {
    console.log('='.repeat(60) + '\n');
    return;
  }

  console.log('Top 10 Whales by Volume:');
  console.log('-'.repeat(60));
  
  // Sort whales by total volume and get top 10
  const topWhales = Array.from(this.#whaleAddresses.values())
    .sort((a, b) => b.totalVolume - a.totalVolume)
    .slice(0, 10);

  topWhales.forEach((whale, index) => {
    const shortAddr = `${whale.address.slice(0, 8)}...${whale.address.slice(-6)}`;
    console.log(`${index + 1}. ${shortAddr}`);
    console.log(`   Volume: $${whale.totalVolume.toLocaleString()}`);
    console.log(`   Trades: ${whale.tradeCount}`);
    console.log(`   Markets: ${Array.from(whale.coins).join(', ')}`);
    console.log(`   Last Active: ${whale.lastSeen.toLocaleString()}\n`);
  });

  console.log('='.repeat(60) + '\n');
}
```

**Statistics include:**

* Total number of whale trades detected
* Number of unique whale addresses
* Top 10 whales ranked by volume
* Individual whale profiles with activity metrics

#### 17. Create the Start Method

```javascript
async start() {
  console.log('🚀 Starting Hyperliquid Whale Tracker...');
  console.log(`📊 Tracking: ${CONFIG.TRACKED_SYMBOLS.join(', ')}`);
  console.log(`💰 Whale threshold: $${CONFIG.WHALE_THRESHOLD_USD.toLocaleString()}\n`);
  
  // Test Telegram if configured
  if (CONFIG.TELEGRAM_BOT_TOKEN && CONFIG.TELEGRAM_CHAT_ID) {
    console.log('✅ Telegram notifications: ENABLED');
    console.log(`📱 Chat ID: ${CONFIG.TELEGRAM_CHAT_ID}`);
    console.log(`📦 Batch mode: ${CONFIG.TELEGRAM_BATCH_ALERTS ? `ON (every ${CONFIG.TELEGRAM_BATCH_INTERVAL/1000}s)` : 'OFF'}\n`);
    
    await this.#testTelegramConnection();
  } else {
    console.log('⚠️  Telegram notifications: DISABLED\n');
  }
  
  // Connect to Hyperliquid
  await this.connectHyperliquid();
  
  // Optional: Connect to HyperEVM
  if (CONFIG.HYPEREVM_WSS) {
    await this.connectHyperEVM();
  } else {
    console.log('ℹ️  HyperEVM WebSocket not configured (optional)\n');
  }

  // Print statistics every 5 minutes
  setInterval(() => this.#printWhaleStats(), 5 * 60 * 1000);
}

stop() {
  this.#ws?.close();
  this.#wsHyperEVM?.close();
  console.log('Bot stopped');
}
```

**Startup sequence:**

1. Display configuration
2. Test Telegram connection
3. Connect to Hyperliquid WebSocket
4. Optionally connect to HyperEVM
5. Start periodic statistics reporting

#### 18. Initialize and Run the Bot

```javascript
// Create bot instance
const bot = new HyperliquidWhaleTracker();
await bot.start();

// Handle graceful shutdown
process.on('SIGINT', () => {
  console.log('\n👋 Shutting down...');
  bot.stop();
  process.exit(0);
});
```

**Proper shutdown:**

* Closes WebSocket connections properly
* Prevents data loss or hanging connections

#### Running and Testing the Application

1. Open your terminal and run the following command:

```bash
node index.js
```

2. Expected result in console:

```
🐋 WHALE TRADE ALERT! 🐋

🟢 BUY AVAX

💰 Size: 56.00 contracts
💵 Price: 17.88
📊 Value: 1,001.392
👤 Trader: Unknown

📝 TX Hash: 0xc806e65080fb77e5c980042f145c7302024000361bfe96b76bcf91a33fff51d0
🆔 Trade ID: 720410117733902
🕐 Time: 11/8/2025, 12:07:11 PM

🐋 WHALE TRADE ALERT! 🐋

🔴 SELL HYPE

💰 Size: 61.56 contracts
💵 Price: 41.46
📊 Value: 2,552.154
👤 Trader: Unknown
📝 TX Hash: 0x03f399453dfc2d42056d042f145c12020235002ad8ff4c14a7bc4497fcf0072c
🆔 Trade ID: 438223268535475
🕐 Time: 11/8/2025, 12:07:04 PM

    

🐳 WHALE WALL DETECTED! 🐳

📈 ETH Orderbook
🟢 BID: $3442.20 × 47.29 = $162,787.146
🟢 BID: $3442.10 × 182.11 = $626,825.342
🔴 ASK: $3444.20 × 336.14 = $1,157,739.243
🔴 ASK: $3444.30 × 64.56 = $222,347.131
🔴 ASK: $3444.40 × 119.15 = $410,398.538

🕐 11/8/2025, 12:07:16 PM
    
^C
👋 Shutting down ...
Bot stopped
```

3. Expected result in Telegram:

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

### Troubleshooting

You may run into a **WebSocket 503** Error:

```
Connecting to GetBlock WebSocket...
WebSocket error: Error: Unexpected server response: 503
WebSocket connection closed
Reconnecting in 16 seconds... (Attempt 4)
```

* This means that your WebSocket endpoint is incorrect.  Check your `.env` file and ensure it's in this pattern:

```bash
# correct format
HYPEREVM_WSS=wss://go.getblock.io/<ACCESS_TOKEN>
```

Then, restart the server.&#x20;

```bash
node index.js
```

### Conclusion

In this guide, you have successfully learn and built a **Hyperliquid Whale Tracker Bot** that monitors and alerts all whale trade activities — from major buys to large sell-offs — and even detects whale walls in the order book.

By integrating the **GetBlock WebSocket API**, you can rest assured of **fast, reliable, and real-time data delivery**, ensuring you never miss significant market movements.



