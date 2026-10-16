---
description: Learn how to deploy a smart contract on Rollux using GetBlock's RPC endpoint.
---

# Deploy smart contract on Rollux

Rollux is EVM-equivalent OP Stack, so contracts deploy with the standard Solidity toolchain — Foundry, Hardhat, or Remix — pointed at a GetBlock endpoint, with no chain-specific compiler or plugin required. Gas is paid in SYS, and each transaction also pays a small OP Stack L1 data-availability fee on top of L2 execution gas. This guide covers adding the network to a wallet and deploying a first contract to Rollux.

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager, or [Foundry](https://book.getfoundry.sh/)
* An EVM wallet (such as MetaMask) with SYS on Rollux for gas — see [Add Network to Your Wallet](deploy-smart-contract-on-rollux.md#add-network-to-your-wallet) and [Funding Your Deployer](deploy-smart-contract-on-rollux.md#funding-your-deployer)
* A GetBlock access token — the endpoint is `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with SYS on Rollux

## Network Details

| Property        | Value                                               |
| --------------- | --------------------------------------------------- |
| Network Name    | Rollux                                              |
| RPC URL         | https://shared.eu-central-1.getblock.io//           |
| Chain ID        | 570 (0x23a)                                         |
| Currency Symbol | SYS                                                 |
| Block Explorer  | [explorer.rollux.com](https://explorer.rollux.com/) |

## Add Network to Your Wallet

Rollux is not pre-configured in MetaMask by default, so it must be added manually — follow the steps below:

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

Enter the values from the [Network Details](deploy-smart-contract-on-rollux.md#network-details) table above.
{% endstep %}

{% step %}
### Confirm the Chain ID

Confirm the Chain ID resolves to `570`.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added Rollux network.
{% endstep %}
{% endstepper %}

## Funding Your Deployer

Rollux gas is paid in SYS.

* **Mainnet** — bridge SYS to Rollux through the official Rollux bridge, or withdraw to a Rollux address from an exchange that supports it.

## Deploy with Foundry

{% stepper %}
{% step %}
### Install Foundry and initialize

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
forge init hello-rollux && cd hello-rollux
```
{% endstep %}

{% step %}
### Write the contract

{% code title="src/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, Rollux";

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
export ROLLUX_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

forge create src/Hello.sol:Hello \
  --rpc-url $ROLLUX_RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```
{% endstep %}

{% step %}
### Verify on the block explorer

Rollux's explorer is Blockscout-based, so verification uses the Blockscout verifier:

```bash
forge verify-contract <DEPLOYED_ADDRESS> src/Hello.sol:Hello \
  --chain-id 570 \
  --verifier blockscout \
  --verifier-url https://explorer.rollux.com/api/
```
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create the project

```bash
mkdir hello-rollux && cd hello-rollux
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

const ROLLUX_CHAIN_ID = 570;

module.exports = {
  solidity: '0.8.24',
  networks: {
    rollux: {
      url: process.env.ROLLUX_RPC_URL,
      chainId: ROLLUX_CHAIN_ID,
      accounts: [process.env.PRIVATE_KEY]
    }
  },
  etherscan: {
    apiKey: { rollux: 'empty' },
    customChains: [
      {
        network: 'rollux',
        chainId: ROLLUX_CHAIN_ID,
        urls: {
          apiURL: 'https://explorer.rollux.com/api',
          browserURL: 'https://explorer.rollux.com'
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
    string public greeting = "Hello, Rollux";

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
export ROLLUX_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

npx hardhat ignition deploy ./ignition/modules/Hello.js --network rollux
```
{% endstep %}

{% step %}
### Verify on the block explorer

```bash
npx hardhat verify --network rollux <DEPLOYED_ADDRESS>
```
{% endstep %}
{% endstepper %}
