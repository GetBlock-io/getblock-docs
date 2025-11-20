---
description: >-
  A complete guide on how to track Pump.fun token launches on Solana in
  real-time using GetBlock’s Yellowstone gRPC service.
icon: rocket-launch
---

# Tracking Pump.fun Token Mints in Real Time with GetBlock’s Yellowstone gRPC

### Overview

Traditional blockchain monitoring relies on polling RPC endpoints every few seconds, which introduces significant latency (2-5 seconds) and checking for updates that may not exist.&#x20;

GetBlock's Yellowstone gRPC service solves this by streaming data directly from Solana validators in real-time.&#x20;

Here is a comparison:

| Traditional RPC Polling                                       | Websocket                                               | Yellowstone gRPC                                                      |
| ------------------------------------------------------------- | ------------------------------------------------------- | --------------------------------------------------------------------- |
| 2-5 second delays, constant API requests, high resource usage | 1-3 second delays, server pushes updates when available | \~400ms latency, bidirectional streaming, direct validator connection |

#### Why GetBlock’s Yellowstone gRPC?

GetBlock provides access to Solana's Yellowstone Geyser gRPC plugin, delivering real-time blockchain data through a high-performance streaming protocol:

* **Ultra-Low Latency:** Direct streaming from validators with \~400ms latency vs 2-5 seconds with traditional RPC polling
* **Efficient Filtering:** Subscribe only to the specific programs and accounts you need, reducing bandwidth and processing overhead
* **Bidirectional Streaming:** Single persistent connection handles both requests and data flow, eliminating polling waste
* **No Infrastructure Needed:** GetBlock manages validators, updates, and monitoring across global data centers (Europe, USA, Asia)
* **Simple Integration:** Standard HTTPS endpoint with token authentication, works with existing gRPC client libraries.

In this guide, you will learn how to:&#x20;

1. Connects to GetBlock's Yellowstone gRPC service
2. Subscribes to all transactions involving Pump.fun's program
3. Filters for token creation instructions specifically
4. Parses instruction data to extract token metadata
5. Extracts account addresses (mint, creator, bonding curve)
6. Displays formatted output with Solscan explorer links
7. Tracks statistics and auto-reconnects on errors

### Prerequisites

Before you begin, ensure you have:

* [Node.js ](https://nodejs.org/en) and npm installed
* Basic knowledge of JavaScript&#x20;
* A [GetBlock’s account](https://account.getblock.io/)
* A Dedicated Solana Node subscription on GetBlock - Required for Yellowstone gRPC access

#### Technology Stack:

* Node.js
* [@triton-one/yellowstone-grpc](https://www.npmjs.com/package/@triton-one/yellowstone-grpc) - Official Yellowstone gRPC client
* [bs58](https://www.npmjs.com/package/bs58) - Base58 encoding for Solana addresses
* GetBlock’s Dedicated Solana Node with Yellowstone add-on

### Step 1: Set Up Your GetBlock’s Yellowstone Endpoint

#### Deploy Your Dedicated Solana Node

First, you need to deploy a dedicated Solana node on GetBlock with the Yellowstone gRPC add-on enabled.

1\. **Sign up or log in**

* Go to[ GetBlock](https://getblock.io/)
* Create an account or log in to your existing account

2\. **Deploy your dedicated node**

* Navigate to your user Dashboard
* Switch to the "**Dedicated nodes**" tab
* Scroll down to "**My endpoints"**
* Under "**Protocol**", select Solana
* Set the network to **Mainnet**
* Click **Get**

![](../.gitbook/assets/unknown.png)

3\. **Enable the Yellowstone gRPC add-on**

* In Step 3 of your node setup (Select API and Add-ons)
* Check the box for **Yellowstone gRPC** under Add-ons
* Complete payout and finalise the setup

![](<../.gitbook/assets/unknown (1).png>)

#### Generate a Yellowstone gRPC Access Token

Once your node is live, you'll create an access token to authenticate your gRPC connections.

1\. **Return to your dashboard**

* Under your dedicated node dashboard, click on  "**My endpoints".**&#x20;
* Select **Yellowstone gRPC**
* Click on **Add** to generate an access token

![](<../.gitbook/assets/unknown (2).png>)

2\. **Your endpoint URL**

You'll receive an HTTPS-style gRPC endpoint URL based on your chosen region:

{% tabs %}
{% tab title="Europe (Frankfurt)" %}
```bash
https://go.getblock.io/YOUR_ACCESS_TOKEN/
```
{% endtab %}

{% tab title="USA (New York)" %}
```bash
https://go.getblock.us/YOUR_ACCESS_TOKEN/
```
{% endtab %}

{% tab title="Asia (Singapore)" %}
```bash
https://go.getblock.asia/YOUR_ACCESS_TOKEN/
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
Ensure you securely store both the base endpoint and the access token. You'll need them in your code to authenticate with GetBlock's Yellowstone service.
{% endhint %}

{% hint style="info" %}
GetBlock provides a single TLS endpoint - you don't need to configure different ports or protocols. Everything works over standard HTTPS (`port 443`).
{% endhint %}

### Step 2: Set up Development Environment

1. Create a directory for your project

```
mkdir pumpfun-monitor
cd pumpfun-monitor
npm init-y
```

2. Install Dependencies:

```bash
npm install @triton-one/yellowstone-grpc bs58@5.0.0
```

What these packages do:

* [@triton-one/yellowstone-grpc](https://www.npmjs.com/package/@triton-one/yellowstone-grpc) - Official client for connecting to Yellowstone gRPC services (works with GetBlock)
* [bs58@5.0.0 ](https://www.npmjs.com/package/bs58)- Encodes binary data to base58 format (Solana's address format). Version 5.0.0 supports vanilla js.

{% hint style="info" %}
`bs58@5.0.0`  because version 6.x + uses ES modules only, which don't work with vanilla js - `require()` statements.
{% endhint %}

#### Project Structure

Create the following files to have a basic structure for your project:

```bash
├── pumpfun-monitor.js          // Main apllication
└── .env                // Environment variables
└── .gitignore          // Git ignore file
```

### Step 3: Start Building Your Monitor

1. Import dependencies into `pumpfun-monitor.js` :

```javascript
const Client = require("@triton-one/yellowstone-grpc").default;
const { CommitmentLevel } = require("@triton-one/yellowstone-grpc");
const bs58 = require("bs58");
```

**What this does:**

* yellowstone gRPC Client for connecting to GetBlock
* `CommitmentLevel` Settings for choosing transaction confirmation speed
* `bs58` for converting binary data into human-readable Solana addresses.

2. Add your GetBlock configuration:&#x20;

```javascript
// GetBlock Configuration
const ENDPOINT = "https://go.getblock.io";  // Your region's endpoint
const TOKEN = "YOUR_ACCESS_TOKEN";           // Your generated token
```

**What this does:**&#x20;

* Stores your GetBlock credentials.&#x20;

{% hint style="danger" %}
Replace **YOUR\_ACCESS\_TOKEN** with the actual token you generated in Step 1. &#x20;

If you chose a different region, use that endpoint instead (e.g., `https://go.getblock.us/<ACCESS_TOKEN>` or `https://go.getblock.asia/<ACCESS_TOKEN>`.
{% endhint %}

3. Add Pump.fun constants:

```javascript
// Pump.fun Program Constants
const PUMPFUN_PROGRAM = "6EF8rrecthR5Dkzon8Nwu78hRvfCKubJ14M5uBEwF6P";
const CREATE_DISCRIMINATOR = Buffer.from([24, 30, 200, 40, 5, 28, 7, 119]);
```

**What this does:**

* &#x20;`PUMPFUN_PROGRAM` - The unique program ID for Pump.fun on Solana
* `CREATE_DISCRIMINATOR` - The 8-byte identifier that marks token creation instructions (derived from hashing "global:create")

4. Add statistics tracker:

```javascript
// Statistics Tracking
let stats = {
  startTime: Date.now(),
  totalMints: 0,
  lastMintTime: null
};
What this does: 
```

* This creates an object to track how many tokens you've detected and when the last one appeared.

#### Step 4: Build Data Parser Functions

Now, you'll add a function to decode the binary data from Pump.fun's instruction format.&#x20;

1. Add string decoder:&#x20;

```javascript
function decodeString(buffer, offset) {
  const length = buffer.readUInt32LE(offset);
  const str = buffer.slice(offset + 4, offset + 4 + length).toString('utf8');
  return { value: str, bytesRead: 4 + length };
}
```

**What this does:**&#x20;

* Pump.fun stores strings as \[4 bytes for length]\[UTF-8 string data]. This function reads that format and returns both the string value and how many bytes were consumed.

2.  Add Instruction Parser: \


    ```javascript
    function parseCreateInstruction(data) {
      try {
        let offset = 8; // Skip the discriminator
        
        const name = decodeString(data, offset);
        offset += name.bytesRead;
        
        const symbol = decodeString(data, offset);
        offset += symbol.bytesRead;
        
        const uri = decodeString(data, offset);
        
        return {
          name: name.value,
          symbol: symbol.value,
          uri: uri.value
        };
      } catch (error) {
        return null;
      }
    }

    ```

    \
    **What this does:**&#x20;

* Reads the token's metadata from Pump.fun's instruction data format. The data is organized like this:
  * **First 8 bytes:** Discriminator (identifies this as a "create" instruction)
  * **Token name** (e.g., "Super Doge")
  * **Token symbol** (e.g., "SDOGE")
  * **Metadata URI** (link to token image and details)

{% hint style="success" %}
The function starts at byte 8 (after the discriminator) and reads each string one by one using the decodeString helper function.
{% endhint %}

### Step 5: Add Account Address Extractor

The next step is to create a function that extracts account addresses to identify newly created tokens' mint address, bonding curve pool address and creator's wallet address.

<pre class="language-javascript"><code class="lang-javascript">function extractAccounts(transaction, instructionIndex) {
  try {
    const message = transaction.transaction.message;
    const instruction = message.instructions[instructionIndex];
    const accountKeys = message.accountKeys;
    
    const accountIndices = Array.from(instruction.accounts);
    const accounts = accountIndices.map(idx => {
      const accountKey = accountKeys[idx];
      return bs58.encode(Buffer.from(accountKey));
    });
    
    return {
<strong>      mint: accounts[0],
</strong><strong>      bondingCurve: accounts[2],
</strong><strong>      creator: accounts[7]
</strong>    };
  } catch (error) {
    return null;
  }
}
</code></pre>

**What this does:**&#x20;

* Extracts three key addresses from the instruction:
  * accounts\[0] - The newly created token's mint address
  * accounts\[2] - The bonding curve pool address
  * accounts\[7] - The creator's wallet address

{% hint style="success" %}
These are defined by Pump.fun's program structure.
{% endhint %}

### Step 6: Add Instruction Verifier

The next step is to verify the instruction:&#x20;

```javascript
function isCreateInstruction(instruction, accountKeys) {
  const programIdx = instruction.programIdIndex;
  const programId = bs58.encode(accountKeys[programIdx]);
  
  if (programId !== PUMPFUN_PROGRAM) return false;
  
  const data = Buffer.from(instruction.data);
  return data.slice(0, 8).equals(CREATE_DISCRIMINATOR);
}
```

**What this does:**&#x20;

* Verifies two things:
  * The instruction calling Pump.fun's program
  * The first 8 bytes which match the create discriminator

{% hint style="success" %}
Only instructions passing both checks are token creations.
{% endhint %}

### Step 7: Add Display Function

The next step is to add a function that displays the data in a readable format:&#x20;

```javascript
function displayTokenMint(tokenData, signature, slot) {
  stats.totalMints++;
  stats.lastMintTime = new Date();
  
  console.log("\n" + "=".repeat(80));
  console.log(`🎉 NEW PUMP.FUN TOKEN MINT #${stats.totalMints}`);
  console.log("=".repeat(80));
  
  console.log(`\n📛 NAME:        ${tokenData.name}`);
  console.log(`🏷️  SYMBOL:      $${tokenData.symbol}`);
  console.log(`\n🪙 MINT:        ${tokenData.mint}`);
  console.log(`👤 CREATOR:     ${tokenData.creator}`);
  console.log(`📊 BONDING:     ${tokenData.bondingCurve}`);
  console.log(`\n🔗 METADATA:    ${tokenData.uri}`);
  console.log(`📜 SIGNATURE:   ${signature}`);
  console.log(`🎰 SLOT:        ${slot}`);
  
  console.log(`\n🔍 EXPLORE:`);
  console.log(`   Token:   https://solscan.io/token/${tokenData.mint}`);
  console.log(`   TX:      https://solscan.io/tx/${signature}`);
  console.log(`   Creator: https://solscan.io/account/${tokenData.creator}`);
  
  console.log("\n" + "=".repeat(80) + "\n");
}
```

\
**What this does:**&#x20;

* Formats and displays all token information in a readable format
* Updates statistics
* Includes Solscan link to the token, transaction hash and creator.

### Step 8: Build the Main Monitor Function

Now you'll create the core function that connects to GetBlock and processes blockchain data.&#x20;

{% hint style="warning" %}
This section is broken into smaller parts for clarity.&#x20;
{% endhint %}

#### Part A: Start the Function and Connect

```javascript
async function monitorPumpfunMints() {
  console.log("🚀 Starting Pump.fun Token Mint Monitor");
  console.log(`🎯 Watching program: ${PUMPFUN_PROGRAM}\n`);
  console.log("Waiting for new token mints...\n");
  
  return new Promise(async (resolve, reject) => {
    try {
      const client = new Client(ENDPOINT, TOKEN, undefined);
      const stream = await client.subscribe();
```

**What this does:**&#x20;

* Creates a client connection to GetBlock using your credentials and opens a bi-directional streaming connection.

#### Part B: Configure Subscription

```javascript
 const request = {
        accounts: {},
        slots: {},
        transactions: {
          pumpfun: {
            accountInclude: [PUMPFUN_PROGRAM],
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

**What this does:**&#x20;

* Tells GetBlock you only want transactions involving Pump.fun's program, with CONFIRMED commitment level (\~2-3 seconds, balanced speed/reliability).

#### Part C: Handle Incoming Data

```javascript
stream.on("data", (message) => {
      try {
        if (message.pong) {
          stream.write({ ping: { id: message.pong.id } });
          return;
        }
        
        if (message.transaction && message.filters && 
            message.filters.includes('pumpfun')) {
          
          const tx = message.transaction.transaction;
          const signature = bs58.encode(tx.signature);
          const slot = message.transaction.slot.toString();
          
          const txMessage = tx.transaction.message;
          const accountKeys = txMessage.accountKeys;
          const instructions = txMessage.instructions;
```

\
What this does:&#x20;

* Processes each message from GetBlock - responds to keep-alive pings and extracts transaction data.

#### Part D: Process Instructions

```javascript
for (let i = 0; i < instructions.length; i++) {
              const instruction = instructions[i];
              
              if (isCreateInstruction(instruction, accountKeys)) {
                const instructionData = Buffer.from(instruction.data);
                const tokenMetadata = parseCreateInstruction(instructionData);
                
                if (!tokenMetadata) continue;
                
                const accounts = extractAccounts(
                  { transaction: { message: txMessage } },
                  i
                );
                
                if (!accounts) continue;
                
                displayTokenMint(
                  { ...tokenMetadata, ...accounts },
                  signature,
                  slot
                );
              }
            }
          }
        } catch (error) {
          console.error(`Error: ${error.message}`);
        }
      });

```

&#x20;What this does:&#x20;

* For each instruction, it checks if it's a create, parses metadata, extracts addresses, and displays results.

#### Part E: Handle Errors

```javascript
    stream.on("error", (error) => {
        console.error(`Stream error: ${error.message}`);
        reject(error);
      });
      
      stream.on("end", () => resolve());
      stream.on("close", () => resolve());
```

What this does:&#x20;

* It handles stream errors and connection closures.

#### Part F: Send Request

```javascript
stream.write(request, (err) => {
        if (err) {
          reject(err);
        } else {
          console.log("✅ Subscription active - monitoring blockchain...\n");
        }
      });
      
    } catch (error) {
      reject(error);
    }
  });
}

```

What this does:&#x20;

* Sends your subscription request to GetBlock and confirms the connection is active.&#x20;

{% hint style="warning" %}
This completes the `monitorPumpfunMints()` function - everything above is part of this one function in your `pumpfun-monitor.js` file.
{% endhint %}

### Step 9: Add Execution Code

Finally, add the code to run your monitor:

```javascript
async function main() {
  try {
    await monitorPumpfunMints();
  } catch (error) {
    console.error("Monitor crashed:", error.message);
    console.log("Restarting in 5 seconds...");
    setTimeout(main, 5000);
  }
}
```

What this does:&#x20;

* Runs your monitor and automatically restarts it if it crashes.

```javascript
process.on('SIGINT', () => {
  console.log("\n\n🛑 Shutting down...");
  console.log(`\nTotal mints detected: ${stats.totalMints}`);
  const uptime = Math.floor((Date.now() - stats.startTime) / 1000);
  console.log(`Uptime: ${Math.floor(uptime / 60)}m ${uptime % 60}s\n`);
  process.exit(0);
});

main();
```

What this does:&#x20;

* Handles `Ctrl+C`  to show final statistics before shutdown.

### Step 10: Run Your Monitor

Run the following command in your terminal to start the server:

```bash
node pumpfun-monitor.js
```

#### Expected Output

You'll see:

```bash
🚀 Starting Pump.fun Token Mint Monitor
🎯 Watching program: 6EF8rrecthR5Dkzon8Nwu78hRvfCKubJ14M5uBEwF6P
Waiting for new token mints...
✅ Subscription active - monitoring blockchain...
```

This means you're connected to GetBlock and monitoring is active.&#x20;

#### When a Token Appears

When a new token appears, it displays like this:

```bash
================================================================================
🎉 NEW PUMP.FUN TOKEN MINT #1
================================================================================

📛 NAME:        Super Doge
🏷️  SYMBOL:      $SDOGE

🪙 MINT:        7xKXtG5CJYAuGZ3w8bXRGLynAmj4twABC123def456
👤 CREATOR:     9wFFyRfZBsuAha4YcuxcXLKwMxJR43S7fPfQLusDBzvT
📊 BONDING:     HMU77nPMBMZ9Wf9TpvvJtFHkjNLkNeGZ8xzBvakKpump

🔗 METADATA:    https://ipfs.io/ipfs/QmYJZ9HGVgYiRGkF...
📜 SIGNATURE:   5TYqzsc8kGY7vMGDZxEbBFWGnEUPTQQMpk9k...
🎰 SLOT:        376205017

🔍 EXPLORE:
   Token:   https://solscan.io/token/7xKXtG5...
   TX:      https://solscan.io/tx/5TYqzsc8kGY7v...
   Creator: https://solscan.io/account/9wFFyRfZ...

================================================================================

```

Congratulations 🎉, you've successfully created an application that tracks Pump.fun token minted.&#x20;

### Troubleshooting

1. **Connection issues:**&#x20;

```
Connection refused
```

This means there is a connection glitch:&#x20;

* Verify your **ENDPOINT** and **TOKEN** in `pumpfun-monitor.js`
* Check your GetBlock dashboard to confirm if the node is active
* Test your internet connection

2. **Tokens not showing:**

* Be patient - Pump.fun typically has 50-200 tokens per day
* Visit[ pump.fun](https://pump.fun/) to verify tokens are being created
* Try `CommitmentLevel.PROCESSED` for faster updates

3. **Parser Errors:**

```bash
Failed to extract accounts
```

&#x20;These warnings are normal. This means some transactions aren't structured.&#x20;

### Security Best Practices

This is highly recommended to use environment variables instead of hard-coding. Create an `.env` file and store your token like this:

```javascript
GETBLOCK_TOKEN=your_token node pumpfun-monitor.js
```

and reference it in your `pumpfun-monitor.js` like this:

```javascript
//use dotenv
const TOKEN = process.env.GETBLOCK_TOKEN;
```

{% hint style="warning" %}
Remember to add the `.env` file in your `.gitignore` file.
{% endhint %}

### Conclusion

In this guide, you learn how to build a tracking application that monitors Pump.fun token minted using GetBlock Yellowstone gRPC. This guide explains the importance of using GetBlock Yellowstone gRPC, how to get your Yellowstone gRPC token, set up the application to get the expected result.&#x20;

It also explains how to troubleshoot some errors you may encounter and possible ways to enhance the security and maintainability of your applications.&#x20;

#### Additional Resources

* [GetBlock Dashboard](https://getblock.io/dashboard/)
* [GetBlock Yellowstone gRPC API](../add-ons/yellowstone-grpc-api/)
* [Yellowstone gRPC GitHub](https://github.com/rpcpool/yellowstone-grpc)
* [Pump.fun on Solscan](https://solscan.io/account/6EF8rrecthR5Dkzon8Nwu78hRvfCKubJ14M5uBEwF6P)

\
\
\
