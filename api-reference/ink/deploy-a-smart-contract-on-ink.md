---
description: Learn how to deploy a smart contract on Ink using GetBlock's RPC endpoint.
---

# Deploy a smart contract on Ink

Ink is EVM-equivalent, so contracts deploy with the standard Solidity toolchain — Foundry, Hardhat, or Remix — pointed at a GetBlock endpoint, with no chain-specific compiler or plugin required. Gas is paid in ETH, as on Ethereum. This guide covers adding the network to a wallet and deploying a first contract to Ink.

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager, or [Foundry](https://book.getfoundry.sh/)
* An EVM wallet (such as MetaMask) with ETH on Ink for gas — see [Add Network to Your Wallet](deploy-a-smart-contract-on-ink.md#add-network-to-your-wallet) and [Funding Your Deployer](deploy-a-smart-contract-on-ink.md#funding-your-deployer)
* A GetBlock access token — the endpoint is `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with ETH on Ink

## Network Details

| Property        | Value                                                       |
| --------------- | ----------------------------------------------------------- |
| Network Name    | Ink                                                         |
| RPC URL         | https://shared.eu-central-1.getblock.io//                   |
| Chain ID        | 57073 (0xdef1)                                              |
| Currency Symbol | ETH                                                         |
| Block Explorer  | [explorer.inkonchain.com](https://explorer.inkonchain.com/) |

## Add Network to Your Wallet

Ethereum Mainnet is pre-configured in MetaMask by default. Ink is not, so it must be added manually — follow the steps below:

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

Enter the values from the [Network Details](deploy-a-smart-contract-on-ink.md#network-details) table above.
{% endstep %}

{% step %}
### Confirm the Chain ID

Confirm the Chain ID resolves to `57073`.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added Ink network.
{% endstep %}
{% endstepper %}

## Funding Your Deployer

Ink gas is paid in ETH.

* **Mainnet** — bridge ETH into Ink through the official Ink bridge, or acquire it on an exchange that supports Ink withdrawals, then send it to your deployer's address.
* **Testnet (Ink Sepolia, chain ID 763373)** — claim test ETH from the Ink Sepolia faucet; see [docs.inkonchain.com](https://docs.inkonchain.com/) for the current faucet and endpoints.

## Deploy with Foundry

{% stepper %}
{% step %}
### Install Foundry and initialize

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
forge init hello-ink && cd hello-ink
```
{% endstep %}

{% step %}
### Write the contract

{% code title="src/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, Ink";

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
export INK_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

forge create src/Hello.sol:Hello \
  --rpc-url $INK_RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```
{% endstep %}

{% step %}
### Verify on the block explorer

Ink's explorer is Blockscout-based, so verification uses the Blockscout verifier:

```bash
forge verify-contract <DEPLOYED_ADDRESS> src/Hello.sol:Hello \
  --chain-id 57073 \
  --verifier blockscout \
  --verifier-url https://explorer.inkonchain.com/api/
```
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create the project

```bash
mkdir hello-ink && cd hello-ink
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

const INK_CHAIN_ID = 57073;

module.exports = {
  solidity: '0.8.24',
  networks: {
    ink: {
      url: process.env.INK_RPC_URL,
      chainId: INK_CHAIN_ID,
      accounts: [process.env.PRIVATE_KEY]
    }
  },
  etherscan: {
    apiKey: { ink: 'empty' },
    customChains: [
      {
        network: 'ink',
        chainId: INK_CHAIN_ID,
        urls: {
          apiURL: 'https://explorer.inkonchain.com/api',
          browserURL: 'https://explorer.inkonchain.com'
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
    string public greeting = "Hello, Ink";

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
export INK_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

npx hardhat ignition deploy ./ignition/modules/Hello.js --network ink
```
{% endstep %}

{% step %}
### Verify on the block explorer

```bash
npx hardhat verify --network ink <DEPLOYED_ADDRESS>
```
{% endstep %}
{% endstepper %}
