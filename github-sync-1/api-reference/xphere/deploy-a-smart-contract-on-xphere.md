---
description: Learn how to deploy a smart contract on Xphere using GetBlock's RPC endpoint.
---

# Deploy a Smart Contract on Xphere

Xphere is EVM-compatible, so contracts deploy with the standard Solidity toolchain — Foundry, Hardhat, or Remix — pointed at a GetBlock endpoint, with no chain-specific compiler or plugin required. Gas is paid in XP. This guide covers adding the network to a wallet and deploying a first contract.

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager, or [Foundry](https://book.getfoundry.sh/)
* An EVM wallet (such as MetaMask) with XP for gas — see [Add Network to Your Wallet](deploy-a-smart-contract-on-xphere.md#add-network-to-your-wallet) and [Funding Your Deployer](deploy-a-smart-contract-on-xphere.md#funding-your-deployer)
* A GetBlock access token — the endpoint is `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with XP

## Network Details

| Property        | Value                                     |
| --------------- | ----------------------------------------- |
| Network Name    | Xphere Mainnet                            |
| RPC URL         | https://shared.eu-central-1.getblock.io// |
| Chain ID        | 20250217 (0x134fe69)                      |
| Currency Symbol | XP                                        |
| Block Explorer  | [xpscan.io](https://xpscan.io/)           |

## Add Network to Your Wallet

Xphere is not pre-configured in MetaMask by default, so it must be added manually — follow the step below:

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

Enter the values from the [Network Details](deploy-a-smart-contract-on-xphere.md#network-details) table above.
{% endstep %}

{% step %}
### Confirm the Chain ID

Confirm the Chain ID resolves to `20250217`.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added Xphere network.
{% endstep %}
{% endstepper %}

## Funding Your Deployer

Xphere gas is paid in XP.

* **Mainnet** — acquire XP from an exchange, then send it to your deployer's address.

## Deploy with Foundry

{% stepper %}
{% step %}
### Install Foundry and initialize

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
forge init hello-xphere && cd hello-xphere
```
{% endstep %}

{% step %}
### Write the contract

{% code title="src/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, Xphere";

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
export RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

forge create src/Hello.sol:Hello \
  --rpc-url $RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```
{% endstep %}

{% step %}
### Verify on the block explorer

Xphere's explorer is Blockscout-based, so verification uses the Blockscout verifier:

```bash
forge verify-contract <DEPLOYED_ADDRESS> src/Hello.sol:Hello \
  --chain-id 20250217 \
  --verifier blockscout \
  --verifier-url https://xpscan.io/api
```
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create the project

```bash
mkdir hello-xphere && cd hello-xphere
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

const CHAIN_ID = 20250217;

module.exports = {
  solidity: '0.8.24',
  networks: {
    xphere: {
      url: process.env.RPC_URL,
      chainId: CHAIN_ID,
      accounts: [process.env.PRIVATE_KEY]
    }
  },
  etherscan: {
    apiKey: { xphere: 'empty' },
    customChains: [
      {
        network: 'xphere',
        chainId: CHAIN_ID,
        urls: {
          apiURL: 'https://xpscan.io/api',
          browserURL: 'https://xpscan.io'
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
    string public greeting = "Hello, Xphere";

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
export RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

npx hardhat ignition deploy ./ignition/modules/Hello.js --network xphere
```
{% endstep %}

{% step %}
### Verify on the block explorer

```bash
npx hardhat verify --network xphere <DEPLOYED_ADDRESS>
```
{% endstep %}
{% endstepper %}

## After Deploying

* Read contract state through the `eth_call` method against your GetBlock endpoint
* Watch contract events with `eth_getLogs`
* Explore your contract and transactions on the [Xphere explorer](https://xpscan.io/)
