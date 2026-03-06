---
description: >-
  Learn how to build an AI agent that swap token, checks balance and transfers
  on Solana
icon: message-bot
---

# How to Build an AI Agent On Solana

AI agents are autonomous systems that perform operations or tasks without human intervention.  These agents act independently, as set up by users.  For example, if you set up your agent to send a reminder or a report to your friend, this agent will perform the task on your behalf without your friend knowing that the action was performed by itself.&#x20;

In the blockchain context, AI agents can:

* Execute token swaps when prices hit targets
* Rebalance portfolios across DeFi protocols
* Monitor wallets and automate transfers
* Mint NFTs based on social media triggers

### Core Stack of AI Agents

Just like every area of development has its own stack for building applications, AI agent development has its own stack, typically consisting of three layers:

1. **Large Language Models (LLMs):** The reasoning brain of every AI agent.  LMs handle natural language understanding, interpret user requests, process blockchain data, and decide which actions to take.
2. **Agent Frameworks:** These define the agent's structure, personality, and interactions with external platforms.  Examples include ElizaOS, LangChain, Rig, and AgentiPy.
3. **Blockchain Toolkits:** SDKs that enable agents to interact with networks like Solana.  Examples include the Solana Agent Kit and GOAT Toolkit.

### Why Are AI Agents Important in Blockchain?

1. **Simplification of Complex Tasks:** AI agents can handle complex DeFi tasks, such as finding the best yield, rebalancing portfolios, or executing arbitrage trades faster than humans.
2. **Enhanced Security:** They provide continuous monitoring of vulnerabilities in smart contracts and wallets and detect fraudulent activity in real time.
3. **Simplified User Experience (Web3 Adoption):** AI agents act as intermediaries, translating natural-language requests into complex on-chain actions, reducing technical barriers for users.
4. **Decentralized Autonomous Interactions:** Agents can hold funds, sign transactions, and interact with dApps autonomously, creating new, self-sustaining, AI-driven economic ecosystems.
5. **Intelligent Data Processing:** They provide an insightful report on vast, immutable datasets on the blockchain to support better decision-making.&#x20;

_**Now that you've learnt about AI agents, their core stack, and its importance.  In this guide, you will be learning how to build an AI agent on Solana, a fast and growing blockchain network.  This agent will be able to transfer  SOL to USDC and check your account balance as seen below:**_

<figure><img src="../.gitbook/assets/Screenshot 2026-03-06 at 3.33.04 PM.png" alt=""><figcaption></figcaption></figure>

### Prerequisites

You must have the following:

1. [Node.js v23+](https://nodejs.org/en/download) is installed on your laptop
2. Solana Wallet, e.g., [Phantom](https://phantom.com/), [Solflare](https://www.solflare.com/), [MetaMask](https://metamask.io/), etc
3. API keys such as [OpenAI](https://platform.openai.com/api-keys) (for the LLM) and Solana Devnet RPC URL (Optional: [GetBlock Solana RPC URL](https://account.getblock.io/))
4. Package manager installed on your laptop(Preferrably [pnpm](https://pnpm.io/installation))

{% hint style="info" %}
Devnet RPC URL is used for this guide. If you want to interact with Mainnet, then make use of [GetBlock Solana RPC URL](https://account.getblock.io/)
{% endhint %}

## Project Setup

{% stepper %}
{% step %}
Create a new project directory

```bash
mkdir solana-ai-agent
cd solana-ai-agent
```
{% endstep %}

{% step %}
Install the dependencies:

{% code overflow="wrap" %}
```bash
pnpm add @goat-sdk/adapter-langchain @goat-sdk/core @goat-sdk/plugin-jupiter @goat-sdk/plugin-pumpfun @goat-sdk/plugin-spl-token @goat-sdk/wallet-solana @langchain/core @langchain/langgraph @langchain/activitylanareal timebs58 dotenv
```
{% endcode %}

<details>

<summary>Packages Description</summary>

| **Package**                 | **What it does**                                                            |
| --------------------------- | --------------------------------------------------------------------------- |
| @goat-sdk/core              | The brain of GOAT — connects your wallet to AI-usable tools                 |
| @goat-sdk/wallet-solana     | Lets GOAT read and control your Solana wallet                               |
| @goat-sdk/plugin-jupiter    | Allows the agent to swap tokens using Jupiter DEX aggregator                |
| @goat-sdk/plugin-spl-token  | Lets the agent send, receive, and check balances of SPL tokens (USDC, BONK) |
| @goat-sdk/plugin-pumpfun    | Lets the agent buy and sell tokens on Pump.fun                              |
| @goat-sdk/adapter-langchain | Converts GOAT tools into a format LangChain agents can call                 |
| @langchain/core             | Core building blocks for the LangChain AI framework                         |
| @langchain/langgraph        | Runs the agent loop — handles thinking, tool calling, and replying          |
| @langchain/openai           | Connects LangChain to OpenAI so GPT-4o can power the reasoning              |
| @solana/web3.js             | Standard library for talking to the Solana blockchain and RPCs              |
| bs58                        | Decodes base58 private keys into a format the code can use                  |
| dotenv                      | Loads your `.env` file so secrets are not hardcoded in the source           |

</details>
{% endstep %}

{% step %}
Create a `.env` file and add the following details:

{% code overflow="wrap" %}
```
RPC_URL=https://api.devnet.solana.com
PRIVATE_KEY=your_wallet_private_key
OPENAI_API_KEY=your_openai_secret_key
```
{% endcode %}
{% endstep %}

{% step %}
Create `agent.ts` file, this is where you will be building your AI agent
{% endstep %}

{% step %}
Import your dependencies:

{% code overflow="wrap" %}
```typescript
import { Connection, Keypair } from "@solana/web3.js";
import { solana } from "@goat-sdk/wallet-solana";
import { jupiter } from "@goat-sdk/plugin-jupiter";
import { splToken } from "@goat-sdk/plugin-spl-token";
import { getOnChainTools } from "@goat-sdk/adapter-langchain";
import { ChatOpenAI } from "@langchain/openai";
import { pumpfun } from "@goat-sdk/plugin-pumpfun";
import { createReactAgent } from "@langchain/langgraph/prebuilt";
import { MemorySaver } from "@langchain/langgraph";
import * as dotenv from "dotenv";
import bs58 from "bs58";
import * as readline from "readline";


dotenv.config();
```
{% endcode %}
{% endstep %}

{% step %}
Initialize your Agent

{% code overflow="wrap" %}
```typescript
async function initializeAgent() {
  const connection = new Connection(process.env.RPC_URL!, "confirmed");
  const privateKey = process.env.SOLANA_PRIVATE_KEY!;
  const keypair = Keypair.fromSecretKey(bs58.decode(privateKey));
  const tools = await getOnChainTools({
    wallet: solana({
      keypair,
      connection,
    }),
    plugins: [
      splToken(),  
      pumpfun(),
      jupiter(),
    ],
  });
  const llm = new ChatOpenAI({
    modelName: "gpt-4o",
    temperature: 0, // Lower temperature is better for tool calling
  });
  const memory = new MemorySaver();
  return createReactAgent({
    llm,
    tools,
    checkpointSaver: memory,
  });
}
```
{% endcode %}

This:

* Connect the AI agent to Solana Devnet and access your wallet to perform actions
* Empower the agent with tools for swapping(`jupiter()`), launch token(`pumpfun()`) and send and receive SPL token(`splToken()`)
* Specify the language model to use
* Create short-term memory so it remembers previous messages within the same chat session.
{% endstep %}

{% step %}
Create Interface for interaction:&#x20;

{% code overflow="wrap" %}
```typescript
async function run() {
  const agent = await initializeAgent();
  const config = { configurable: { thread_id: "goat-session-1" } };
  const rl = readline.createInterface({ input: process.stdin, output: process.stdout });
  const ask = (prompt: string) => new Promise<string>((resolve) => rl.question(prompt, resolve)
  console.log('Solana AI Agent ready. Type "exit" to quit.\n');
  while (true) {
    const input = await ask("You: ");
    if (input.trim().toLowerCase() === "exit") {
      rl.close();
      break;
    }
    const response = await agent.invoke(
      { messages: [{ role: "user", content: input }] },
      config,
    );
    const lastMessage = response.messages[response.messages.length - 1];
    console.log("Agent:", lastMessage.content, "\n");
  }
}
run().catch(console.error);
```
{% endcode %}

This defines an interactive CLI chat loop between you and the AI agent and also logs any errors.
{% endstep %}

{% step %}
Run the agent using this command

{% code overflow="wrap" %}
```bash
npx tsx agent.ts
```
{% endcode %}

You can make any request in any language, as seen below:

{% code overflow="wrap" %}
```bash
You: What is my wallet address and how much sol do i have left
Agent: Your wallet address is `5C7dUe5ZDWBCYQ5eqtGPRyz7CQYk2aK4FhZTDYQeji7r`, and you have approximately 7.94991 SOL left in your wallet. 

You: Send 0.05 sol to 3xk5f92EhRfDSD29W9ZWKmX2x7DXkc6TpMW8TpX5eFU8
Agent: The transaction to send 0.05 SOL to the address `3xk5f92EhRfDSD29W9ZWKmX2x7DXkc6TpMW8TpX5eFU8` has been successfully completed. You can view the transaction details with the hash: [2ZK5U6pJ7i6j8nDRvdidgEyPnRPivzyBddUrJGebnefApFFXcvmomYNGZVC85XNjnAG5PTnuUyCwFYtjp55HNfLX](https://explorer.solana.com/tx/2ZK5U6pJ7i6j8nDRvdidgEyPnRPivzyBddUrJGebnefApFFXcvmomYNGZVC85XNjnAG5PTnuUyCwFYtjp55HNfLX). 

You: Если вы обращаетесь в поддержку, можно сказать: "Подскажите, пожалуйста, мой адрес кошелька и баланс SOL"
Agent: Ваш адрес кошелька: `5C7dUe5ZDWBCYQ5eqtGPRyz7CQYk2aK4FhZTDYQeji7r`.

К сожалению, возникла ошибка при попытке получить баланс SOL. Пожалуйста, проверьте правильность адреса кошелька и попробуйте снова. 

```
{% endcode %}

You can see that it interprets your natural language, uses the tools plugin to get the right response for it&#x20;
{% endstep %}
{% endstepper %}

#### Extend AI Agent capacity

&#x20;Improve the capacity of your agent by adding more tools:

1. Mint NFT:

<pre class="language-typescript" data-overflow="wrap"><code class="lang-typescript"><strong>import { crossmint } from "@goat-sdk/crossmint";
</strong>import { Connection, Keypair } from "@solana/web3.js";
import { solana } from "@goat-sdk/wallet-solana";
import { jupiter } from "@goat-sdk/plugin-jupiter";
import { splToken } from "@goat-sdk/plugin-spl-token";
import { getOnChainTools } from "@goat-sdk/adapter-langchain";
import { ChatOpenAI } from "@langchain/openai";
import { pumpfun } from "@goat-sdk/plugin-pumpfun";
import { createReactAgent } from "@langchain/langgraph/prebuilt";
import { MemorySaver } from "@langchain/langgraph";
import * as dotenv from "dotenv";
import bs58 from "bs58";
import * as readline from "readline";

async function initializeAgent() { .....

// continuation
const tools = await getOnChainTools({
    wallet: solana({
      keypair,
      connection,
    }),
    plugins: [
      splToken(),  
      pumpfun(),
      jupiter(),
<strong>      crossmint()
</strong>    ],
  });
}
/// rest of code (readline interface and agent.invoke loop)

</code></pre>

### Best Practice

1. Store private keys in `.env` only — never commit this file.
2. Use a dedicated burner wallet with minimal funds for testing
3. On mainnet, double-check all transactions before confirming.

### Conclusion

In this guide, you have learn what AI agents are, their core stack, and how to build one for yourself to send, swap, launch a token, and check your account balance.

### Additional Resources

1. [GOAT Docs](https://github.com/goat-sdk/goat)
2. [Langchain Docs](https://docs.langchain.com/)
3. [OpenAI Platform](https://platform.openai.com/)
4. [GetBlock Account Dashboard](https://account.getblock.io/)



