---
description: >-
  How to Build an App to Track Large Solana Transactions with GetBlock
  Yellowstone Geyer gRPC
icon: tv-retro
---

# Listening to High-Value SOL Transactions via Yellowstone Geyser gRPC with GetBlock

Monitoring high-value SOL transactions is essential for understanding the dynamics of the Solana network. By using tools like Yellowstone Geyser gRPC with GetBlock, developers can gain real-time insights into significant financial movements, enabling them to make informed decisions and capitalize on trends within the Solana ecosystem.

In this tutorial, you'll build a monitoring app that detects high-value SOL transfers the moment they occur on the Solana blockchain.

You'll learn how to:

* Connect to GetBlock's Yellowstone gRPC service for ultra-low latency blockchain data streaming
* Subscribe to Solana's System Program to capture all native SOL transfers
* Parse transfer instruction data to extract amounts, sender addresses, and recipient addresses
* Filter transactions based on transfer amount thresholds
* Display real-time alerts with formatted output and explorer links
* Track statistics, including total volume and largest transfers

#### Technology Stack:

* Node.js&#x20;
* [@triton-one/yellowstone-grpc](https://www.npmjs.com/package/@triton-one/yellowstone-grpc) - Official Yellowstone gRPC client
* [bs58](https://www.npmjs.com/package/bs58) - Base58 encoding for Solana addresses
* GetBlock's Dedicated Solana Node with Yellowstone add-on

### Prerequisites

Before you begin, ensure you have:

* Node.js v18 or higher is installed on your system
* Basic JavaScript knowledge
* A GetBlock account - Free sign-up available at getblock.io
* A Dedicated Solana Node subscription on GetBlock - Required for Yellowstone gRPC access

### Step 1: Set Up Your GetBlock Yellowstone Endpoint

#### Deploy Your Dedicated Solana Node

First, you need to deploy a dedicated Solana node on GetBlock with the Yellowstone gRPC add-on enabled.

1\. Sign up or log in

* Go to GetBlock
* Create an account or log in to your existing account

2\. Deploy your dedicated node

* Navigate to your user Dashboard
* Switch to the "**Dedicated nodes**" tab
* Scroll down to "**My endpoints.**"
* Under "Protocol", select Solana
* Set the network to Mainnet
* Click Get

3\. Enable the Yellowstone gRPC add-on

* In Step 3 of your node setup (Select API and Add-ons)
* Check the box for Yellowstone gRPC under Add-ons
* Complete payout and finalize the setup

#### Generate Your gRPC Access Token

Once your node is live, you'll create an access token to authenticate your gRPC connections.

1\. Return to your dashboard

* Go to "My endpoints" in your Dedicated node dashboard and generate a gRPC Access Token

2\. Your endpoint URL

You'll receive an HTTPS-style gRPC endpoint URL based on your chosen region:

{% tabs %}
{% tab title="Singapore" %}
```bash
https://go.getblock.asia/YOUR_ACCESS_TOKEN/
```
{% endtab %}

{% tab title="New York" %}
```bash
https://go.getblock.us/YOUR_ACCESS_TOKEN/
```
{% endtab %}

{% tab title="Frankfurt" %}
```bash
https://go.getblock.io/YOUR_ACCESS_TOKEN/
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
Save both pieces of information:

* Base Endpoint: https://go.getblock.io (or your region)
* Access Token: The alphanumeric token generated

You'll use these in your code to authenticate with GetBlock's Yellowstone service.
{% endhint %}

### Step 2: Initialize Your Project

1. Open your terminal and create a new directory:

```bash
mkdir sol-transfer-monitor
cd sol-transfer-monitor
```

2. Initialize Node.js Project

```bash
npm init -y
```

3. Install the required packages:

```bash
npm install @triton-one/yellowstone-grpc bs58@5.0.0 dotenv
```

3. Create a new file:

```bash
touch sol-transfer-monitor.js
```

3. Configure `package.json`:

```json
{
  "name": "sol-transfer-monitor",
  "version": "1.0.0",
  "description": "A Listener that tracks high-value SOL transactions",
  "main": "sol-transfer-monitor.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "node sol-transfer-monitor.js"
  },
  "keywords": [
    "Solana",
    "Liquidity Monitor",
    "Onchain transfer"
  ],
  "author": "",
  "license": "ISC",
  "type": "module",
  "dependencies": {
    "@triton-one/yellowstone-grpc": "^4.0.2",
    "bs58": "^5.0.0",
    "dotenv": "^17.2.3"
  }
}

```

* `"type": "module"` - Enables ES6 import/export syntax (required for modern JavaScript)
* `"main": "sol-transfer-monitor.js"` - Specifies the entry point of your application
* `"scripts"` - Defines shortcuts like `npm start`

### Project Structure

1. Create the following files to have a basic structure for your project:

```bash
pumpfun-migrations-listener/
├── sol-transfer-monitor.js          # Main application logic
├── .env              # Environment variables (add to .gitignore)
├── .gitignore        # Git ignore file
└── package.json      # Project configuration
```

2. Create a `.gitignore` file:

```bash
node_modules/
.env
*.log
```

### Step 3: Start Building Your Monitor

{% hint style="info" %}
You'll build everything in a single file called `sol-transfer-monitor.js.`
{% endhint %}

#### Import Dependencies

```bash
import Client from "@triton-one/yellowstone-grpc";
import { CommitmentLevel } from "@triton-one/yellowstone-grpc";
import bs58 from "bs58";
import { config } from "dotenv";
config();

```

What this does:

* `Client` - The main class for establishing a connection to GetBlock's Yellowstone gRPC service
* `CommitmentLevel`- Configuration options that determine how finalized transactions should be before you receive them (PROCESSED = fastest but can be rolled back, CONFIRMED = balanced, FINALIZED = slowest but guaranteed)
* `bs58` - A library that converts Solana's binary address format into the human-readable base58 strings you see on blockchain explorers (like "9wFFyRfZBsuAha4YcuxcXLKwMxJR...")
* `config`: This loads the `.env` file

#### Add Your GetBlock Configuration

```javascript
const ENDPOINT = "https://go.getblock.io";  // Your region's endpoint
const TOKEN = process.env.GETBLOCK_TOKEN;           // Your generated token
```

What this does: Stores your GetBlock credentials for authenticating your connection.

{% hint style="warning" %}
If you chose a different region during setup, use that base URL instead (e.g., https://go.getblock.us or https://go.getblock.asia).
{% endhint %}

#### Add Solana System Program Constants

```javascript
// System Program (handles all native SOL transfers)
const SYSTEM_PROGRAM = "11111111111111111111111111111111";

// Minimum transfer amount in SOL to alert on
const MIN_TRANSFER_AMOUNT = 100; // 100 SOL threshold
```

What this does:

* `SYSTEM_PROGRAM` - This is the unique identifier for Solana's built-in System Program, which is responsible for all native SOL transfers on the network. Every time someone sends SOL from one wallet to another, this program processes it.
* `MIN_TRANSFER_AMOUNT` - Sets the threshold for what qualifies as a "high-value" transfer. Only transfers of 100 SOL or more will trigger an alert. You can adjust this to any amount (e.g., 50 SOL, 1000 SOL, etc.).

#### Add Statistics Tracker

```js
// Statistics Tracking
let stats = {
  startTime: Date.now(),
  totalTransfers: 0,
  totalVolume: 0,
  largestTransfer: 0,
  largestTransferTx: null
};
```

What this does:&#x20;

* Creates an object to track key metrics about the transfers you're monitoring. It records:&#x20;
  * When the monitor started
  * How many high-value transfers have been detected
  * the cumulative volume of all transfers
  * information about the largest transfer seen so far.

### Step 4: Build Utility Functions

Now you'll add helper functions to format data and display results.

#### Add SOL Formatter

```javascript
function formatSOL(lamports) {
  return (lamports / 1_000_000_000).toFixed(4);
}
```

What this does:&#x20;

* Converts lamports (Solana's smallest unit) to SOL for human-readable display. Solana stores all amounts in lamports, where 1 SOL equals 1 billion lamports (similar to how 1 Bitcoin = 100 million satoshis). This function divides by 1 billion and rounds to 4 decimal places, so 5,500,000,000 lamports becomes "5.5000 SOL".

#### Add Display Function

```javascript
function displayTransfer(transferData) {
  stats.totalTransfers++;
  stats.totalVolume += transferData.amount;
  
  if (transferData.amount > stats.largestTransfer) {
    stats.largestTransfer = transferData.amount;
    stats.largestTransferTx = transferData.signature;
  }
  
  const solAmount = formatSOL(transferData.amount);
  
  console.log("\n" + "=".repeat(80));
  console.log(`HIGH VALUE TRANSFER #${stats.totalTransfers}`);
  console.log("=".repeat(80));
  
  console.log(`\nAMOUNT:      ${solAmount} SOL`);
  console.log(`FROM:        ${transferData.from}`);
  console.log(`TO:          ${transferData.to}`);
  console.log(`SIGNATURE:   ${transferData.signature}`);
  console.log(`SLOT:        ${transferData.slot}`);
  
  console.log(`\nEXPLORE:`);
  console.log(`   TX:     https://solscan.io/tx/${transferData.signature}`);
  console.log(`   From:   https://solscan.io/account/${transferData.from}`);
  console.log(`   To:     https://solscan.io/account/${transferData.to}`);
  
  console.log("\n" + "=".repeat(80) + "\n");
}
```

\
What this does:&#x20;

* Formats and displays all transfer information clearly. It first updates your running statistics (incrementing the transfer count, adding to total volume, and checking if this is the largest transfer seen).&#x20;
* Then it converts the amount from lamports to SOL and prints out all the details, including sender, recipient, transaction signature, and slot number.&#x20;
* Finally, it includes clickable Solscan explorer links so you can investigate the transaction, sender, and recipient in more detail.

### Step 5: Build the Transfer Parser

Now you'll create the function that extracts transfer details from instruction data.

#### Add Transfer Instruction Parser

```javascript
function parseTransferInstruction(instruction, accountKeys) {
  try {
    const data = Buffer.from(instruction.data);
    
    // System program transfer instruction has type 2
    // Data format: [4 bytes: instruction type (2)][8 bytes: lamports amount]
    if (data.length < 12) return null;
    
    const instructionType = data.readUInt32LE(0);
    if (instructionType !== 2) return null; // Not a transfer instruction
    
    const lamports = data.readBigUInt64LE(4);
    
    // Get from and to accounts
    const accountIndices = Array.from(instruction.accounts);
    if (accountIndices.length < 2) return null;
    
    const fromAccount = bs58.encode(Buffer.from(accountKeys[accountIndices[0]]));
    const toAccount = bs58.encode(Buffer.from(accountKeys[accountIndices[1]]));
    
    return {
      amount: Number(lamports),
      from: fromAccount,
      to: toAccount
    };
  } catch (error) {
    return null;
  }
}
```

\
What this does:&#x20;

* Extracts the transfer amount and account addresses from a System Program instruction.

{% hint style="success" %}
If anything goes wrong during parsing (malformed data, missing accounts, etc.), it catches the error and returns null.
{% endhint %}

### Step 6: Build the Main Monitor Function

Now you'll create the core function that connects to GetBlock and processes blockchain data.

#### Part A: Start the Function and Connect

```javascript
async function monitorHighValueTransfers() {
  console.log("Starting High Value SOL Transfer Monitor");
  console.log(`Minimum amount: ${MIN_TRANSFER_AMOUNT} SOL`);
  console.log(`Watching: Native SOL transfers\n`);
  console.log("Waiting for high value transfers...\n");
  
  return new Promise(async (resolve, reject) => {
    try {
      const client = new Client(ENDPOINT, TOKEN, undefined);
      const stream = await client.subscribe();
```

What this does:&#x20;

* Initializes the monitor by displaying startup information and establishing a connection to GetBlock.
* Creates a new Client instance using your endpoint and token, then opens a bidirectional streaming connection.&#x20;
* The `subscribe()` method returns a stream object that handles both sending requests to GetBlock and receiving blockchain data.

#### Part B: Configure Subscription

```javascript
 const request = {
        accounts: {},
        slots: {},
        transactions: {
          sol_transfers: {
            accountInclude: [SYSTEM_PROGRAM],
            accountExclude: [],
            accountRequired: []
          }
        },
        transactionsStatus: {},
        entry: {},
        blocks: {},
        blocksMeta: {},
        commitment: CommitmentLevel.CONFIRMED,
        accountsDataSlice: [],
        ping: undefined
      };

```

What this does:&#x20;

* Builds the subscription request that tells GetBlock exactly what blockchain data you want to receive.   &#x20;
  * It filters sol\_transfers
  * Send transactions that interact with the system
  * Only receive transactions that are CONFIRMED&#x20;

#### Part C: Handle Incoming Data

```javascript
stream.on("data", (message) => {
    try {
      if (message.pong) {
        stream.write({ ping: { id: message.pong.id } });
        return;
      }
      
      if (message.transaction && message.filters && 
          message.filters.includes('sol_transfers')) {
        
        const tx = message.transaction.transaction;
        const signature = bs58.encode(tx.signature);
        const slot = message.transaction.slot.toString();
        
        const txMessage = tx.transaction.message;
        const accountKeys = txMessage.accountKeys;
        const instructions = txMessage.instructions;

```

\
What this does:&#x20;

* Sets up an event handler that processes every message GetBlock sends through the stream.

#### Part D: Process Instructions

```javascript
// Process each instruction
    for (const instruction of instructions) {
      const programIdx = instruction.programIdIndex;
      const programId = bs58.encode(accountKeys[programIdx]);
      
      // Only process System Program instructions
      if (programId !== SYSTEM_PROGRAM) continue;
      
      const transferData = parseTransferInstruction(instruction, accountKeys);
      
      if (!transferData) continue;
      
      // Check if transfer meets minimum threshold
      const solAmount = transferData.amount / 1_000_000_000;
      if (solAmount >= MIN_TRANSFER_AMOUNT) {
        displayTransfer({
          ...transferData,
          signature: signature,
          slot: slot
        });
      }
    }
  }
} catch (error) {
  console.error(`Error processing transaction: ${error.message}`);
}
});

```

What this does:&#x20;

* Loops through each instruction in the transaction to find and process SOL transfers.

#### Part E: Handle Stream Events

```javascript
stream.on("error", (error) => {
        console.error(`Stream error: ${error.message}`);
        reject(error);
      });
      
      stream.on("end", () => resolve());
      stream.on("close", () => resolve());

```

What this does:&#x20;

* Sets up handlers for stream connection issues.&#x20;
* If the stream encounters an error (network problem, authentication issue, etc.), it logs the error message and rejects the Promise, which will trigger the auto-restart logic in the `main()` function.&#x20;
* The "end" and "close" events indicate the stream has been terminated gracefully, so it resolves the Promise normally.

#### Part F: Send Subscription Request

```javascript
stream.write(request, (err) => {
        if (err) {
          reject(err);
        } else {
          console.log("Subscription active - monitoring blockchain...\n");
        }
      });
      
    } catch (error) {
      reject(error);
    }
  });
}

```

\
What this does:&#x20;

* Send your subscription request to GetBlock to activate the stream.&#x20;
* Once the request is sent successfully, it confirms that monitoring is active.&#x20;
* The callback function checks if there was an error sending the request - if so, it rejects the Promise; otherwise, it displays a success message. This completes the `monitorHighValueTransfers()` function.

### Step 7: Add Execution Code

Finally, add the code to run your monitor and handle shutdown.&#x20;

#### Add Main Execution Function

```javascript
async function main() {
  try {
    await monitorHighValueTransfers();
  } catch (error) {
    console.error("Monitor crashed:", error.message);
    console.log("Restarting in 5 seconds...");
    setTimeout(main, 5000);
  }
}
```

\
What this does:&#x20;

* Wraps your monitor in an auto-restart loop. If the monitor crashes for any reason (network disconnection, API error, unexpected data format), it catches the error, displays an error message, waits 5 seconds, and then automatically restarts the monitor. This ensures your monitor keeps running even if temporary issues occur.

#### Add Shutdown Handler

```javascript
process.on('SIGINT', () => {
  console.log("\n\nShutting down...");
  console.log(`\nTotal high value transfers: ${stats.totalTransfers}`);
  console.log(`Total volume: ${formatSOL(stats.totalVolume)} SOL`);
  console.log(`Largest transfer: ${formatSOL(stats.largestTransfer)} SOL`);
  if (stats.largestTransferTx) {
    console.log(`Largest TX: https://solscan.io/tx/${stats.largestTransferTx}`);
  }
  const uptime = Math.floor((Date.now() - stats.startTime) / 1000);
  console.log(`Uptime: ${Math.floor(uptime / 60)}m ${uptime % 60}s\n`);
  process.exit(0);
});
```

\
What this does:&#x20;

* Handles shutdown when you press `Ctrl+C` or `cmd+C`. Instead of abruptly terminating, it displays a summary of your monitoring session, including total transfers detected, cumulative volume, the largest transfer amount with a link to view it, and how long the monitor was running. Then it cleanly exits the process.

#### Start the Monitor

```javascript
main();
```

What this does:&#x20;

* Run everything. This is the last line of your sol-transfer-monitor.js file and triggers the execution of your monitor.

### TestStep 8: Tets Your Monitor

1\. Start the Monitor

```bash
npm start
```

2. Expected Output:

```bash
> sol-transfer-monitor@1.0.0 start
> node sol-transfer-monitor.js

[dotenv@17.2.3] injecting env (1) from .env -- tip: ⚙️  load multiple .env files with { path: ['.env.local', '.env'] }
Starting High Value SOL Transfer Monitor
Minimum amount: 100 SOL
Watching: Native SOL transfers

Waiting for high value transfers...

Subscription active - monitoring blockchain...


================================================================================
HIGH VALUE TRANSFER #1
================================================================================

AMOUNT:      1990.0000 SOL
FROM:        5tzFkiKscXHK5ZXCGbXZxdw7gTjjD1mBwuoFbhUvuAi9
TO:          53AXjkQHZQEbHZvV55VHtcZtFzBzArLZZG5AF4ufkRhv
SIGNATURE:   4uJgnfRohiq6c9CDWUtk3w94ouYGya5dGkx4LSS5tuFGDRypuXtN7cQLLy3eNj5V1b5PwEWccJqLak1Rnd9kRtTB
SLOT:        382899112

EXPLORE:
   TX:     https://solscan.io/tx/4uJgnfRohiq6c9CDWUtk3w94ouYGya5dGkx4LSS5tuFGDRypuXtN7cQLLy3eNj5V1b5PwEWccJqLak1Rnd9kRtTB
   From:   https://solscan.io/account/5tzFkiKscXHK5ZXCGbXZxdw7gTjjD1mBwuoFbhUvuAi9
   To:     https://solscan.io/account/53AXjkQHZQEbHZvV55VHtcZtFzBzArLZZG5AF4ufkRhv

================================================================================

^C

Shutting down...

Total high value transfers: 7
Total volume: 5519.0244 SOL
Largest transfer: 2272.0770 SOL
Largest TX: https://solscan.io/tx/3p92tKFb4AGRUJKA87vJD4fYcb3Z9wKi5AuUskA6TAR5SYciZxL3qmSH88ubQP3g7xSJQjC5kEeMSr7Lhhn8Hkc4
Uptime: 1m 50s
```

### Troubleshooting

1. Permission denied:

```javascript
Stream error: 7 PERMISSION_DENIED: RBAC: access denied
Monitor crashed: 7 PERMISSION_DENIED: RBAC: access denied
```

This means that your access token is missing or incomplete, or you are using the wrong Base URL.&#x20;

2\. No Transfers Showing

* High-value transfers (100+ SOL) are less common than you might expect
* Try lowering MIN\_TRANSFER\_AMOUNT to 10 or 50 SOL to see more activity
* Use CommitmentLevel.PROCESSED for faster updates (though some may be rolled back)

### Conclusion

In this guide, you've learnt how to build a monitor app that tracks high-value SOL transactions on Solana networks. It shows you the steps involved, such as:

* Connects to GetBlock's Yellowstone gRPC service
* Streams blockchain data with \~400ms latency
* Filters for System Program transactions
* Parses SOL transfer amounts and addresses
* Filters by minimum transfer threshold
* Displays formatted results with links
* Tracks cumulative statistics
* Handles errors and auto-reconnects

#### Additional Resources

* [SOL Transfer Monitor Repo](https://github.com/GetBlock-io/guides/tree/main/sol-transfer-monitor)
* [GetBlock Solana RPC Node](https://getblock.io/chains/solana)
* [GetBlock Yellowstone gRPC](../add-ons/yellowstone-grpc-api/)
* [Yellowstone gRPC GitHub](https://github.com/rpcpool/yellowstone-grpc)

<br>
