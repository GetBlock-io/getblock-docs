---
description: Learn how to deploy a smart contract on Soneium using GetBlock's RPC endpoint.
---

# Deploy smart contract - Soneium

Soneium is EVM-equivalent OP Stack, so contracts deploy with the standard Solidity toolchain — Foundry, Hardhat, or Remix — pointed at a GetBlock endpoint, with no chain-specific compiler or plugin required. Gas is paid in ETH, and each transaction also pays a small OP Stack L1 data-availability fee on top of L2 execution gas. This guide covers adding the network to a wallet and deploying a first contract to Soneium.

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager, or [Foundry](https://book.getfoundry.sh/)
* An EVM wallet (such as MetaMask) with ETH on Soneium for gas — see [Add Network to Your Wallet](deploy-smart-contract-soneium.md#add-network-to-your-wallet) and [Funding Your Deployer](deploy-smart-contract-soneium.md#funding-your-deployer)
* A GetBlock access token — the endpoint is `https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with ETH on Soneium

## Network Details

| Property        | Value                                                     |
| --------------- | --------------------------------------------------------- |
| Network Name    | Soneium                                                   |
| RPC URL         | https://shared.us-east-1.getblock.io//                    |
| Chain ID        | 1868 (0x74c)                                              |
| Currency Symbol | ETH                                                       |
| Block Explorer  | [soneium.blockscout.com](https://soneium.blockscout.com/) |

## Add Network to Your Wallet

Soneium is not pre-configured in MetaMask by default, so it must be added manually — follow the step below:

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

Enter the values from the [Network Details](deploy-smart-contract-soneium.md#network-details) table above.
{% endstep %}

{% step %}
### Confirm the Chain ID

Confirm the Chain ID resolves to `1868`.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added Soneium network.
{% endstep %}
{% endstepper %}

## Funding Your Deployer

Soneium gas is paid in ETH, bridged from Ethereum L1.

* **Mainnet** — bridge ETH to Soneium through the official Soneium bridge, or withdraw to a Soneium address from an exchange that supports it.

## Deploy with Foundry

{% stepper %}
{% step %}
### Install Foundry and initialize

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
forge init hello-soneium && cd hello-soneium
```
{% endstep %}

{% step %}
### Write the contract

{% code title="src/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, Soneium";

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
export SONEIUM_RPC_URL=https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

forge create src/Hello.sol:Hello \
  --rpc-url $SONEIUM_RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```
{% endstep %}

{% step %}
### Verify on the block explorer

Soneium's explorer is Blockscout-based, so verification uses the Blockscout verifier:

```bash
forge verify-contract <DEPLOYED_ADDRESS> src/Hello.sol:Hello \
  --chain-id 1868 \
  --verifier blockscout \
  --verifier-url https://soneium.blockscout.com/api/
```
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create the project

```bash
mkdir hello-soneium && cd hello-soneium
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

const SONEIUM_CHAIN_ID = 1868;

module.exports = {
  solidity: '0.8.24',
  networks: {
    soneium: {
      url: process.env.SONEIUM_RPC_URL,
      chainId: SONEIUM_CHAIN_ID,
      accounts: [process.env.PRIVATE_KEY]
    }
  },
  etherscan: {
    apiKey: { soneium: 'empty' },
    customChains: [
      {
        network: 'soneium',
        chainId: SONEIUM_CHAIN_ID,
        urls: {
          apiURL: 'https://soneium.blockscout.com/api',
          browserURL: 'https://soneium.blockscout.com'
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
    string public greeting = "Hello, Soneium";

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
export SONEIUM_RPC_URL=https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

npx hardhat ignition deploy ./ignition/modules/Hello.js --network soneium
```
{% endstep %}

{% step %}
### Verify on the block explorer

```bash
npx hardhat verify --network soneium <DEPLOYED_ADDRESS>
```
{% endstep %}
{% endstepper %}

## After Deploying

* Read contract state through the `eth_call` method against your GetBlock endpoint
* Watch contract events with `eth_getLogs`
* Explore your contract and transactions on the [Soneium explorer](https://soneium.blockscout.com/)
