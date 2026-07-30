---
description: >-
  This guide covers adding the network to a browser wallet, then deploying a
  simple contract using Foundry and Hardhat — pick whichever you prefer.
---

# Deploy Smart Contract On Polygon

Polygon is an EVM-compatible chain, so smart contracts written in Solidity or Vyper deploy without modification using standard tooling: **Foundry**, **Hardhat**, or **Remix**.&#x20;

### Prerequisites

Before deploying, you'll need:

* A wallet with ETH on the target network for gas. See [Add Network to Your Wallet](deploy-smart-contract-on-polygon.md#add-network-to-your-wallet) below.
* A GetBlock access token — sign up at [Dashboard](https://account.getblock.io/) and create an Ethereum endpoint in the dashboard. Every code sample below uses `<ACCESS-TOKEN>` as the placeholder.
* **Node.js 20 or newer** installed (for Hardhat).
* A funded deployer address. For testnets, use the [faucet page](https://getblock.io/faucet/)

### Network Details

| Property        | Ethereum Mainnet                                     | amoy Testnet                                                   |
| --------------- | ---------------------------------------------------- | -------------------------------------------------------------- |
| Chain ID        | 137 (`0x137`)                                        | 80002(0x13882)                                                 |
| RPC URL         | `https://go.getblock.io/<ACCESS-TOKEN>/`             | `https://go.getblock.io/<ACCESS-TOKEN>/`                       |
| Currency Symbol | ETH                                                  | ETH                                                            |
| Block Explorer  | [https://polygonscan.com/](https://polygonscan.com/) | [https://amoy.polygonscan.com/](https://amoy.polygonscan.com/) |

{% hint style="warning" %}
**Deploy to a testnet first.** Every command below defaults to Mainnet — replace the environment variable values with testnet ones for a dry run before touching real ETH.

**Never commit a real private key.** Use environment variables and prefer a throwaway deployer key for testing. Private keys committed to a public repo are drained within seconds by automated bots.
{% endhint %}

### Add Network to Your Wallet

If the automatic button isn't available or the request is rejected, add the network manually:

1. Open MetaMask (or your EVM wallet of choice).
2. Click the network dropdown at the top of the wallet.
3. Click **Add network** → **Add a network manually**.
4. Enter the network details from the [Network Details](deploy-smart-contract-on-polygon.md#network-details) table above.
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
mkdir Poly-deploy && cd Poly-deploy
forge init
```
{% endstep %}

{% step %}
#### Create a contract

Create `src/HelloEthereum.sol` with the following content:

```solidity
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.30;

contract HelloPolygon {
    function hello() external pure returns (string memory) {
        return "Hello, Polygon!";
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
export POLY_RPC_URL=https://go.getblock.io/<ACCESS-TOKEN>/

# Deploy contract
forge create HelloEthereum \
  --rpc-url $POLYGON_RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```

The command prints the deployed contract address on success.
{% endstep %}

{% step %}
#### Verify on Polyscan

Polyscan uses a unified V2 API across all Ethereum networks. Get an API key from [polygonscan.com/api](https://polygonscan.com/api), then:

```bash
# Verify the contract on Polyscan (Mainnet — chain-id 137)
forge verify-contract <contract_address> \
  src/HelloPolygon.sol:HelloPolygon\
  --chain-id 137 \
  --polyscan-api-key $POLYSCAN_API_KEY
```

For Amoy use `--chain-id 80002`;  view your contract at `https://polygonscan.com/address/<contract_address>` (or the corresponding testnet explorer).
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
mkdir poly-deploy && cd poly-deploy
npm init -y
npm install --save-dev hardhat
npx hardhat init
```

> Select **Create a JavaScript project** (or TypeScript if preferred) when prompted.
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
      url: process.env.POLY_RPC_URL,
      chainId: 137,
      accounts: [process.env.PRIVATE_KEY],
    },
    amoy: {
      url: process.env.POLY_RPC_URL,
      chainId: 80002,
      accounts: [process.env.PRIVATE_KEY],
    },
  }
};
```
{% endcode %}

Set your environment variables before proceeding:

```bash
export PRIVATE_KEY=0x<your_private_key>
export POLY_RPC_URL=https://go.getblock.io/<ACCESS-TOKEN>/
export POLYSCAN_API_KEY=<your_polyscan_v2_api_key>
```
{% endstep %}

{% step %}
#### Create a contract

Create `contracts/HelloEthereum.sol`:

```solidity
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.30;

contract HelloPolygon {
    function hello() external pure returns (string memory) {
        return "Hello, Polygon!";
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
  const contract = await hre.ethers.deployContract("HelloPolygon");
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

# Deploy to Amoy (recommended first)
npx hardhat run scripts/deploy.js --network amoy

# Or deploy to Mainnet when ready
npx hardhat run scripts/deploy.js --network mainnet
```
{% endstep %}

{% step %}
#### Verify on Polyscan

```bash
# Verify on the network you deployed to
npx hardhat verify --network amoy <contract_address>
```

After verification, view your contract at the corresponding block explorer:

* Mainnet: `https://polyscan.com/address/<contract_address>`
* Sepolia: `https://amoy.polyscan.com/address/<contract_address>`
{% endstep %}
{% endstepper %}
{% endtab %}
{% endtabs %}
