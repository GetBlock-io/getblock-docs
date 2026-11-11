---
description: Learn how to deploy a smart contract on Story using GetBlock's RPC endpoint.
---

# Deploy a smart contract on Story

Story's execution layer is EVM-equivalent (story-geth), so contracts deploy with the standard Solidity toolchain — Foundry, Hardhat, or Remix — pointed at a GetBlock endpoint, with no chain-specific compiler or plugin required. Gas is paid in DATA. This guide covers adding the network to a wallet and deploying a first contract to Story.

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager, or [Foundry](https://book.getfoundry.sh/)
* An EVM wallet (such as MetaMask) with DATA on Story for gas — see [Add Network to Your Wallet](deploy-a-smart-contract-on-story.md#add-network-to-your-wallet) and [Funding Your Deployer](deploy-a-smart-contract-on-story.md#funding-your-deployer)
* A GetBlock access token — the endpoint is `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with DATA on Story

## Network Details

| Property        | Value                                     |
| --------------- | ----------------------------------------- |
| Network Name    | Story                                     |
| RPC URL         | https://shared.eu-central-1.getblock.io// |
| Chain ID        | 1514 (0x5ea)                              |
| Currency Symbol | DATA                                      |
| Block Explorer  | [storyscan.io](https://www.storyscan.io/) |

## Add Network to Your Wallet

Story is not pre-configured in MetaMask by default, so it must be added manually — follow the steps below:

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

Enter the values from the [Network Details](deploy-a-smart-contract-on-story.md#network-details) table above.
{% endstep %}

{% step %}
### Confirm the Chain ID

Confirm the Chain ID resolves to `1514`.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added Story network.
{% endstep %}
{% endstepper %}

## Funding Your Deployer

Story gas is paid in DATA.

* **Mainnet** — acquire DATA from an exchange or bridge, then send it to your deployer's address.

## Deploy with Foundry

{% stepper %}
{% step %}
### Install Foundry and initialize

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
forge init hello-story && cd hello-story
```
{% endstep %}

{% step %}
### Write the contract

{% code title="src/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, Story";

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
export STORY_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

forge create src/Hello.sol:Hello \
  --rpc-url $STORY_RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```
{% endstep %}

{% step %}
### Verify on the block explorer

Story's explorer is Blockscout-based, so verification uses the Blockscout verifier:

```bash
forge verify-contract <DEPLOYED_ADDRESS> src/Hello.sol:Hello \
  --chain-id 1514 \
  --verifier blockscout \
  --verifier-url https://www.storyscan.io/api/
```
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create the project

```bash
mkdir hello-story && cd hello-story
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

const STORY_CHAIN_ID = 1514;

module.exports = {
  solidity: '0.8.24',
  networks: {
    story: {
      url: process.env.STORY_RPC_URL,
      chainId: STORY_CHAIN_ID,
      accounts: [process.env.PRIVATE_KEY]
    }
  },
  etherscan: {
    apiKey: { story: 'empty' },
    customChains: [
      {
        network: 'story',
        chainId: STORY_CHAIN_ID,
        urls: {
          apiURL: 'https://www.storyscan.io/api',
          browserURL: 'https://www.storyscan.io'
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
    string public greeting = "Hello, Story";

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
export STORY_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

npx hardhat ignition deploy ./ignition/modules/Hello.js --network story
```
{% endstep %}

{% step %}
### Verify on the block explorer

```bash
npx hardhat verify --network story <DEPLOYED_ADDRESS>
```
{% endstep %}
{% endstepper %}

## After Deploying

* Read contract state through the `eth_call` method against your GetBlock endpoint.
* Watch contract events with `eth_getLogs`.
* Explore your contract and transactions on [StoryScan](https://www.storyscan.io/).
