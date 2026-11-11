---
description: Learn how to deploy a smart contract on Goat using GetBlock's RPC endpoint.
---

# Deploy a smart contract on Goat

GOAT Network is EVM-compatible, so contracts deploy with the standard Solidity toolchain — Foundry, Hardhat, or Remix — pointed at a GetBlock endpoint, with no chain-specific compiler or plugin required. The one difference from an Ethereum deploy is that gas is paid in BTC. This guide covers adding the network to a wallet and deploying a first contract to GOAT Network.

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager, or [Foundry](https://book.getfoundry.sh/)
* An EVM wallet (such as MetaMask) with BTC on GOAT Network for gas — see [Add Network to Your Wallet](deploy-a-smart-contract-on-goat.md#add-network-to-your-wallet) and [Funding Your Deployer](deploy-a-smart-contract-on-goat.md#funding-your-deployer)
* A GetBlock access token — the endpoint is `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with BTC on GOAT Network

## Network Details

| Property        | Value                                                   |
| --------------- | ------------------------------------------------------- |
| Network Name    | GOAT Network                                            |
| RPC URL         | https://shared.eu-central-1.getblock.io//               |
| Chain ID        | 2345 (0x929)                                            |
| Currency Symbol | BTC                                                     |
| Block Explorer  | [explorer.goat.network](https://explorer.goat.network/) |

## Add Network to Your Wallet

Ethereum Mainnet is pre-configured in MetaMask by default. GOAT Network is not, so it must be added manually — follow the steps below:

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
### Enter network details

Enter the values from the [Network Details](deploy-a-smart-contract-on-goat.md#network-details) table above.
{% endstep %}

{% step %}
### Confirm the Chain ID

Confirm the Chain ID resolves to `2345`.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added GOAT Network.
{% endstep %}
{% endstepper %}

## Funding Your Deployer

GOAT Network gas is paid in BTC.

* **Mainnet** — bridge BTC into GOAT Network through the official GOAT bridge, or acquire it on an exchange that supports GOAT withdrawals, then send it to your deployer's address.

## Deploy with Foundry

{% stepper %}
{% step %}
### Install Foundry and initialize

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
forge init hello-goat && cd hello-goat
```
{% endstep %}

{% step %}
### Write the contract

{% code title="src/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, GOAT";

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
export GOAT_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

forge create src/Hello.sol:Hello \
  --rpc-url $GOAT_RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```
{% endstep %}

{% step %}
### Verify on the block explorer

GOAT Network's explorer is Blockscout-based, so verification uses the Blockscout verifier:

```bash
forge verify-contract <DEPLOYED_ADDRESS> src/Hello.sol:Hello \
  --chain-id 2345 \
  --verifier blockscout \
  --verifier-url https://explorer.goat.network/api/
```
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create the project

```bash
mkdir hello-goat && cd hello-goat
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

const GOAT_CHAIN_ID = 2345;

module.exports = {
  solidity: '0.8.24',
  networks: {
    goat: {
      url: process.env.GOAT_RPC_URL,
      chainId: GOAT_CHAIN_ID,
      accounts: [process.env.PRIVATE_KEY]
    }
  },
  etherscan: {
    apiKey: { goat: 'empty' },
    customChains: [
      {
        network: 'goat',
        chainId: GOAT_CHAIN_ID,
        urls: {
          apiURL: 'https://explorer.goat.network/api',
          browserURL: 'https://explorer.goat.network'
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
    string public greeting = "Hello, GOAT";

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
export GOAT_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

npx hardhat ignition deploy ./ignition/modules/Hello.js --network goat
```
{% endstep %}

{% step %}
### Verify on the block explorer

```bash
npx hardhat verify --network goat <DEPLOYED_ADDRESS>
```
{% endstep %}
{% endstepper %}

{% hint style="info" %}
GOAT Network's distinctive feature is Bitcoin-native security and BTC-denominated gas. If your application handles BTC deposits or withdrawals across the GOAT bridge, review the GOAT bridge and BitVM2 mechanics in the GOAT documentation before deploying to mainnet.
{% endhint %}
