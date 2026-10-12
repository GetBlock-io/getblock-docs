---
description: >-
  Learn how to deploy a smart contract on Immutable zkEVM using GetBlock's RPC
  endpoint.
---

# Deploy a smart contract Immutable zkEVM

Immutable zkEVM is EVM-equivalent, so contracts deploy with the standard Solidity toolchain — Foundry, Hardhat, or Remix — pointed at a GetBlock endpoint, with no chain-specific compiler or plugin required. The one difference from an Ethereum deploy is that gas is paid in IMX. This guide covers adding the network to a wallet and deploying a first contract to Immutable zkEVM.

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager, or [Foundry](https://book.getfoundry.sh/)
* An EVM wallet (such as MetaMask) with IMX on Immutable zkEVM for gas — see [Add Network to Your Wallet](deploy-a-smart-contract-immutable-zkevm.md#add-network-to-your-wallet) and [Funding Your Deployer](deploy-a-smart-contract-immutable-zkevm.md#funding-your-deployer)
* A GetBlock access token — the endpoint is `https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with IMX on Immutable zkEVM

## Network Details

| Property        | Value                                                     |
| --------------- | --------------------------------------------------------- |
| Network Name    | Immutable zkEVM                                           |
| RPC URL         | https://shared.us-east-1.getblock.io//                    |
| Chain ID        | 13371 (0x343b)                                            |
| Currency Symbol | IMX                                                       |
| Block Explorer  | [explorer.immutable.com](https://explorer.immutable.com/) |

## Add Network to Your Wallet

Ethereum Mainnet is pre-configured in MetaMask by default. Immutable zkEVM is not, so it must be added manually — follow the steps below:

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

Enter the values from the [Network Details](deploy-a-smart-contract-immutable-zkevm.md#network-details) table above.
{% endstep %}

{% step %}
### Confirm the Chain ID

Confirm the Chain ID resolves to `13371`.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added Immutable zkEVM network.
{% endstep %}
{% endstepper %}

## Funding Your Deployer

Immutable zkEVM gas is paid in IMX.

* **Mainnet** — bridge IMX into Immutable zkEVM through the official Immutable bridge, or acquire it on an exchange that supports Immutable zkEVM withdrawals, then send it to your deployer's address.
* **Testnet (Immutable zkEVM Testnet, chain ID 13473)** — claim test IMX from the Immutable zkEVM Testnet faucet; see [docs.immutable.com](https://docs.immutable.com/) for the current faucet and endpoints.

## Deploy with Foundry

{% stepper %}
{% step %}
### Install Foundry and initialize

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
forge init hello-immutable && cd hello-immutable
```
{% endstep %}

{% step %}
### Write the contract

{% code title="src/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, Immutable zkEVM";

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
export IMX_RPC_URL=https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

forge create src/Hello.sol:Hello \
  --rpc-url $IMX_RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```
{% endstep %}

{% step %}
### Verify on the block explorer

Immutable zkEVM's explorer is Blockscout-based, so verification uses the Blockscout verifier:

```bash
forge verify-contract <DEPLOYED_ADDRESS> src/Hello.sol:Hello \
  --chain-id 13371 \
  --verifier blockscout \
  --verifier-url https://explorer.immutable.com/api/
```
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create the project

```bash
mkdir hello-immutable && cd hello-immutable
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

const IMMUTABLE_CHAIN_ID = 13371;

module.exports = {
  solidity: '0.8.24',
  networks: {
    immutable: {
      url: process.env.IMX_RPC_URL,
      chainId: IMMUTABLE_CHAIN_ID,
      accounts: [process.env.PRIVATE_KEY]
    }
  },
  etherscan: {
    apiKey: { immutable: 'empty' },
    customChains: [
      {
        network: 'immutable',
        chainId: IMMUTABLE_CHAIN_ID,
        urls: {
          apiURL: 'https://explorer.immutable.com/api',
          browserURL: 'https://explorer.immutable.com'
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
    string public greeting = "Hello, Immutable zkEVM";

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
export IMX_RPC_URL=https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

npx hardhat ignition deploy ./ignition/modules/Hello.js --network immutable
```
{% endstep %}

{% step %}
### Verify on the block explorer

```bash
npx hardhat verify --network immutable <DEPLOYED_ADDRESS>
```
{% endstep %}
{% endstepper %}

{% hint style="info" %}
Immutable zkEVM is built for games and digital assets, with Immutable's Passport wallet and minting/orderbook APIs layered on top. If you are issuing NFTs or integrating Passport, review Immutable's product SDKs in the Immutable documentation alongside this low-level RPC reference.
{% endhint %}
