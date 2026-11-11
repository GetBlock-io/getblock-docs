---
description: >-
  This guide covers adding the network to a browser wallet, then deploying a
  simple contract using Foundry and Hardhat — pick whichever you prefer.
---

# Deploy Smart Contract On Ethereum

Ethereum is the reference EVM chain, so smart contracts written in Solidity or Vyper deploy without modification using standard tooling: **Foundry**, **Hardhat**, or **Remix**.&#x20;

### Prerequisites

Before deploying, you'll need:

* A wallet with ETH on the target network for gas. See [Add Network to Your Wallet](deploy-smart-contract-on-ethereum.md#add-network-to-your-wallet) below.
* A GetBlock access token — sign up at [Dashboard](https://account.getblock.io/) and create an Ethereum endpoint in the dashboard. Every code sample below uses `<ACCESS-TOKEN>` as the placeholder.
* **Node.js 20 or newer** installed (for Hardhat).
* A funded deployer address. For testnets, use the [faucet page](https://getblock.io/faucet/)

### Network Details

| Property        | Ethereum Mainnet                         | Sepolia Testnet                                       | Hoodi Testnet                                     |
| --------------- | ---------------------------------------- | ----------------------------------------------------- | ------------------------------------------------- |
| Chain ID        | 1 (`0x1`)                                | 11155111 (`0xaa36a7`)                                 | 560048 (`0x88bb0`)                                |
| RPC URL         | `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/` | `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`              | `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`          |
| Currency Symbol | ETH                                      | ETH                                                   | ETH                                               |
| Block Explorer  | [etherscan.io](https://etherscan.io/)    | [sepolia.etherscan.io](https://sepolia.etherscan.io/) | [hoodi.etherscan.io](https://hoodi.etherscan.io/) |

{% hint style="info" %}
Ethereum Mainnet and Sepolia are pre-configured in MetaMask by default. Use the button below only if you want to route MetaMask through GetBlock's RPC endpoint instead of the default provider. **Hoodi is not pre-configured** and must be added manually or via the button below.
{% endhint %}

{% hint style="warning" %}
**Deploy to a testnet first.** Every command below defaults to Mainnet — replace the environment variable values with testnet ones for a dry run before touching real ETH.

**Never commit a real private key.** Use environment variables and prefer a throwaway deployer key for testing. Private keys committed to a public repo are drained within seconds by automated bots.
{% endhint %}

### Add Network to Your Wallet

If the automatic button isn't available or the request is rejected, add the network manually:

1. Open MetaMask (or your EVM wallet of choice).
2. Click the network dropdown at the top of the wallet.
3. Click **Add network** → **Add a network manually**.
4. Enter the network details from the [Network Details](deploy-smart-contract-on-ethereum.md#network-details) table above.
5. Save and switch to the newly added network.

### How to Deploy a Smart Contract

{% tabs %}
{% tab title="Foundry" %}
Foundry is a fast, minimal Solidity toolchain. It's the recommended path if you want deterministic, script-based deployments without any Node.js dependencies.

{% stepper %}
{% step %}
#### Install Foundry

If Foundry is already installed, skip this step:

```bash
# Install foundryup
curl -L https://foundry.paradigm.xyz | bash

# Install forge, anvil, cast, and chisel
foundryup
```
{% endstep %}

{% step %}
#### Create a project

```bash
# Initialize a new project
mkdir eth-deploy && cd eth-deploy
forge init
```
{% endstep %}

{% step %}
#### Create a contract

Create `src/HelloEthereum.sol` with the following content:

```solidity
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.30;

contract HelloEthereum {
    function hello() external pure returns (string memory) {
        return "Hello, Ethereum!";
    }
}
```
{% endstep %}

{% step %}
#### Deploy

Set environment variables, then deploy:

```bash
# Set environment variables
export PRIVATE_KEY=0x<your_private_key>
export ETH_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

# Deploy contract
forge create HelloEthereum \
  --rpc-url $ETH_RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```

The command prints the deployed contract address on success.
{% endstep %}

{% step %}
#### Verify on Etherscan

Etherscan uses a unified V2 API across all Ethereum networks. Get an API key from [etherscan.io/apis](https://etherscan.io/apis), then:

```bash
# Verify the contract on Etherscan (Mainnet — chain-id 1)
forge verify-contract <contract_address> \
  src/HelloEthereum.sol:HelloEthereum \
  --chain-id 1 \
  --etherscan-api-key $ETHERSCAN_API_KEY
```

For Sepolia use `--chain-id 11155111`; for Hoodi use `--chain-id 560048`. After verification, view your contract at `https://etherscan.io/address/<contract_address>` (or the corresponding testnet explorer).
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Hardhat" %}
Hardhat is a JavaScript/TypeScript-based smart contract framework. It's the recommended path if your project already uses Node.js tooling and you want tight integration with test suites, plugins, and TypeScript typings.

{% stepper %}
{% step %}
#### Create a project

Initialize your environment and Hardhat project:

```bash
# Initialize project and install Hardhat
mkdir eth-deploy && cd eth-deploy
npm init -y
npm install --save-dev hardhat
npx hardhat init
```

Select **Create a JavaScript project** (or TypeScript if preferred) when prompted.
{% endstep %}

{% step %}
#### Configure Ethereum networks

Update `hardhat.config.js` with the network settings and Etherscan API for verification:

{% code title="hardhat.config.js" %}
```javascript
require("@nomicfoundation/hardhat-toolbox");

module.exports = {
  solidity: "0.8.30",
  networks: {
    mainnet: {
      url: process.env.ETH_RPC_URL,
      chainId: 1,
      accounts: [process.env.PRIVATE_KEY],
    },
    sepolia: {
      url: process.env.ETH_RPC_URL,
      chainId: 11155111,
      accounts: [process.env.PRIVATE_KEY],
    },
    hoodi: {
      url: process.env.ETH_RPC_URL,
      chainId: 560048,
      accounts: [process.env.PRIVATE_KEY],
    },
  },
  etherscan: {
    // Etherscan V2 unified API — single API key works for all networks
    apiKey: process.env.ETHERSCAN_API_KEY,
  },
};
```
{% endcode %}

Set your environment variables before proceeding:

```bash
export PRIVATE_KEY=0x<your_private_key>
export ETH_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export ETHERSCAN_API_KEY=<your_etherscan_v2_api_key>
```
{% endstep %}

{% step %}
#### Create a contract

Create `contracts/HelloEthereum.sol`:

```solidity
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.30;

contract HelloEthereum {
    function hello() external pure returns (string memory) {
        return "Hello, Ethereum!";
    }
}
```
{% endstep %}

{% step %}
#### Compile and Deploy

Create `scripts/deploy.js`:

{% code title="scripts/deploy.js" %}
```javascript
const hre = require("hardhat");

async function main() {
  const contract = await hre.ethers.deployContract("HelloEthereum");
  await contract.waitForDeployment();
  console.log("Deployed to:", await contract.getAddress());
}

main().catch((error) => {
  console.error(error);
  process.exitCode = 1;
});
```
{% endcode %}

Compile and deploy:

```bash
# Compile
npx hardhat compile

# Deploy to Sepolia (recommended first)
npx hardhat run scripts/deploy.js --network sepolia

# Or deploy to Mainnet when ready
npx hardhat run scripts/deploy.js --network mainnet
```
{% endstep %}

{% step %}
#### Verify on Etherscan

```bash
# Verify on the network you deployed to
npx hardhat verify --network sepolia <contract_address>
```

After verification, view your contract at the corresponding block explorer:

* Mainnet: `https://etherscan.io/address/<contract_address>`
* Sepolia: `https://sepolia.etherscan.io/address/<contract_address>`
* Hoodi: `https://hoodi.etherscan.io/address/<contract_address>`
{% endstep %}
{% endstepper %}
{% endtab %}
{% endtabs %}
