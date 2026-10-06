---
description: Learn how to deploy a smart contract on Corn using GetBlock's RPC endpoint.
---

# Deploy a smart contract on Corn - Corn

Corn is EVM-equivalent, so contracts deploy with the standard Solidity toolchain — Foundry, Hardhat, or Remix — pointed at a GetBlock endpoint, with no chain-specific compiler or plugin required. The one difference from an Ethereum deploy is that gas is paid in BTCN, Corn's Bitcoin-backed native token. This guide covers adding the network to a wallet and deploying a first contract to Corn.

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager, or [Foundry](https://book.getfoundry.sh/)
* An EVM wallet (such as MetaMask) with BTCN on Corn for gas — see [Add Network to Your Wallet](deploy-a-smart-contract-on-corn-corn.md#add-network-to-your-wallet) and [Funding Your Deployer](deploy-a-smart-contract-on-corn-corn.md#funding-your-deployer)
* A GetBlock access token — the endpoint is `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with BTCN on Corn

## Network Details

| Property        | Value                                       |
| --------------- | ------------------------------------------- |
| Network Name    | Corn (Maizenet)                             |
| RPC URL         | `https://shared.eu-central-1.getblock.io//` |
| Chain ID        | 21000000 (0x1406f40)                        |
| Currency Symbol | BTCN                                        |
| Block Explorer  | [cornscan.io](https://cornscan.io/)         |

## Add Network to Your Wallet

Ethereum Mainnet is pre-configured in MetaMask by default. Corn is not, so it must be added manually — follow the steps below:

{% stepper %}
{% step %}
### Open your wallet

Open MetaMask (or your EVM wallet of choice).
{% endstep %}

{% step %}
### Add a custom network

Open the network selector and choose **Add a custom network**.
{% endstep %}

{% step %}
### Enter the network details

Enter the values from the [Network Details](deploy-a-smart-contract-on-corn-corn.md#network-details) table above.
{% endstep %}

{% step %}
### Confirm the Chain ID

Confirm the Chain ID resolves to `21000000`.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added Corn network.
{% endstep %}
{% endstepper %}

## Funding Your Deployer

Corn gas is paid in BTCN, a Bitcoin-backed token.

* **Mainnet** — bridge BTC into Corn to receive BTCN through the official Corn bridge, or acquire BTCN on an exchange that supports Corn withdrawals, then send it to your deployer's address.

## Deploy with Foundry

{% stepper %}
{% step %}
### Install Foundry and initialize

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
forge init hello-corn && cd hello-corn
```
{% endstep %}

{% step %}
### Write the contract

{% code title="src/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, Corn";

    function setGreeting(string calldata greeting_) external {
        greeting = greeting_;
    }
}
```
{% endcode %}
{% endstep %}

{% step %}
### Deploy

```bash
export CORN_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

forge create src/Hello.sol:Hello \
  --rpc-url $CORN_RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```
{% endstep %}

{% step %}
### Verify on the block explorer

Corn's Maizenet explorer is Blockscout-based, so verification uses the Blockscout verifier:

```bash
forge verify-contract <DEPLOYED_ADDRESS> src/Hello.sol:Hello \
  --chain-id 21000000 \
  --verifier blockscout \
  --verifier-url https://maizenet-explorer.usecorn.com/api/
```
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create the project

```bash
mkdir hello-corn && cd hello-corn
npm init --yes
npm install --save-dev hardhat
npx hardhat init
```
{% endstep %}

{% step %}
### Configure the network

{% code title="hardhat.config.js" %}
```javascript
require('@nomicfoundation/hardhat-toolbox');

const CORN_CHAIN_ID = 21000000;

module.exports = {
  solidity: '0.8.24',
  networks: {
    corn: {
      url: process.env.CORN_RPC_URL,
      chainId: CORN_CHAIN_ID,
      accounts: [process.env.PRIVATE_KEY]
    }
  },
  etherscan: {
    apiKey: { corn: 'empty' },
    customChains: [
      {
        network: 'corn',
        chainId: CORN_CHAIN_ID,
        urls: {
          apiURL: 'https://maizenet-explorer.usecorn.com/api',
          browserURL: 'https://maizenet-explorer.usecorn.com'
        }
      }
    ]
  }
};
```
{% endcode %}
{% endstep %}

{% step %}
### Write the contract

{% code title="contracts/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, Corn";

    function setGreeting(string calldata greeting_) external {
        greeting = greeting_;
    }
}
```
{% endcode %}
{% endstep %}

{% step %}
### Deploy

```bash
export CORN_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

npx hardhat ignition deploy ./ignition/modules/Hello.js --network corn
```
{% endstep %}

{% step %}
### Verify on the block explorer

```bash
npx hardhat verify --network corn <DEPLOYED_ADDRESS>
```
{% endstep %}
{% endstepper %}

{% hint style="info" %}
Corn's defining feature is BTCN, its Bitcoin-backed gas token. If your application handles BTCN deposits or withdrawals, review the Corn bridge contracts and BTCN mechanics in the Corn documentation before deploying to mainnet.
{% endhint %}

## After Deploying

* Read contract state through the [eth\_call](/broken/pages/f6e68ce43521bb08e12c6334cec4b2d557eb4576) method against your GetBlock endpoint
* Watch contract events with [eth\_getLogs](/broken/pages/05036408b150af0a10d2f533d3daaf53a0827ad5)
* Explore your contract and transactions on [CornScan](https://cornscan.io/)
