---
description: >-
  Learn how to deploy a smart contract on Matchain using GetBlock's RPC
  endpoint.
---

# Deploy a Smart Contract on Matchain

Matchain is EVM-compatible, so contracts deploy with the standard Solidity toolchain — Foundry, Hardhat, or Remix — pointed at a GetBlock endpoint, with no chain-specific compiler or plugin required. The one difference from an Ethereum deploy is that gas is paid in BNB. This guide covers adding the network to a wallet and deploying a first contract to Matchain.

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager, or [Foundry](https://book.getfoundry.sh/)
* An EVM wallet (such as MetaMask) with BNB on Matchain for gas — see [Add Network to Your Wallet](deploy-a-smart-contract-on-matchain.md#add-network-to-your-wallet) and [Funding Your Deployer](deploy-a-smart-contract-on-matchain.md#funding-your-deployer)
* A GetBlock access token — the endpoint is `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with BNB on Matchain

## Network Details

| Property        | Value                                                    |
| --------------- | -------------------------------------------------------- |
| Network Name    | Matchain                                                 |
| RPC URL         | https://shared.eu-central-1.getblock.io/\<ACCESS-TOKEN>/ |
| Chain ID        | 698 (0x2ba)                                              |
| Currency Symbol | BNB                                                      |
| Block Explorer  | [matchscan.io](https://matchscan.io/)                    |

## Add Network to Your Wallet

Ethereum Mainnet is pre-configured in MetaMask by default. Matchain is not, so it must be added manually — follow the steps below:

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

Enter the values from the [Network Details](deploy-a-smart-contract-on-matchain.md#network-details) table above.
{% endstep %}

{% step %}
### Confirm the Chain ID

Confirm the Chain ID resolves to `698`.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added Matchain network.
{% endstep %}
{% endstepper %}

## Funding Your Deployer

Matchain gas is paid in BNB.

* **Mainnet** — bridge BNB into Matchain through the official Matchain bridge, or acquire it on an exchange that supports Matchain withdrawals, then send it to your deployer's address.
* **Testnet (Matchain Testnet, chain ID 699)** — claim test BNB from the Matchain Testnet faucet; see [docs.matchain.io](https://docs.matchain.io/) for the current faucet and endpoints.

## Deploy with Foundry

{% stepper %}
{% step %}
### Install Foundry and initialize

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
forge init hello-matchain && cd hello-matchain
```
{% endstep %}

{% step %}
### Write the contract

{% code title="src/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, Matchain";

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
export MATCHAIN_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

forge create src/Hello.sol:Hello \
  --rpc-url $MATCHAIN_RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```
{% endstep %}

{% step %}
### Verify on the block explorer

Matchscan is an Etherscan-compatible explorer:

```bash
forge verify-contract <DEPLOYED_ADDRESS> src/Hello.sol:Hello \
  --chain-id 698 \
  --verifier etherscan \
  --verifier-url https://matchscan.io/api \
  --etherscan-api-key <MATCHSCAN_API_KEY>
```
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create the project

```bash
mkdir hello-matchain && cd hello-matchain
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

const MATCHAIN_CHAIN_ID = 698;

module.exports = {
  solidity: '0.8.24',
  networks: {
    matchain: {
      url: process.env.MATCHAIN_RPC_URL,
      chainId: MATCHAIN_CHAIN_ID,
      accounts: [process.env.PRIVATE_KEY]
    }
  },
  etherscan: {
    apiKey: { matchain: process.env.MATCHSCAN_API_KEY },
    customChains: [
      {
        network: 'matchain',
        chainId: MATCHAIN_CHAIN_ID,
        urls: {
          apiURL: 'https://matchscan.io/api',
          browserURL: 'https://matchscan.io'
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
    string public greeting = "Hello, Matchain";

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
export MATCHAIN_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

npx hardhat ignition deploy ./ignition/modules/Hello.js --network matchain
```
{% endstep %}

{% step %}
### Verify on the block explorer

```bash
export MATCHSCAN_API_KEY=your_matchscan_api_key
npx hardhat verify --network matchain <DEPLOYED_ADDRESS>
```
{% endstep %}
{% endstepper %}
