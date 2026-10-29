---
description: Learn how to deploy a smart contract on Zilliqa using GetBlock's RPC endpoint.
---

# Deploy a Smart Contract on Zilliqa

Zilliqa is EVM-compatible, so contracts deploy with the standard Solidity toolchain — Foundry, Hardhat, or Remix — pointed at a GetBlock endpoint, with no chain-specific compiler or plugin required. Gas is paid in ZIL. This guide covers adding the network to a wallet and deploying a first contract.

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager, or [Foundry](https://book.getfoundry.sh/)
* An EVM wallet (such as MetaMask) with ZIL for gas — see [Add Network to Your Wallet](deploy-a-smart-contract-on-zilliqa.md#add-network-to-your-wallet) and [Funding Your Deployer](deploy-a-smart-contract-on-zilliqa.md#funding-your-deployer)
* A GetBlock access token — the endpoint is `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with ZIL

## Network Details

| Property        | Value                                                     |
| --------------- | --------------------------------------------------------- |
| Network Name    | Zilliqa Mainnet                                           |
| RPC URL         | https://shared.eu-central-1.getblock.io//                 |
| Chain ID        | 32769 (0x8001)                                            |
| Currency Symbol | ZIL                                                       |
| Block Explorer  | [zilliqa.blockscout.com](https://zilliqa.blockscout.com/) |

{% hint style="info" %}
Zilliqa has two address formats: the EVM `0x…` form (used by these `eth_*` methods and MetaMask) and the native bech32 `zil1…` form used by the Scilla/native-Zilliqa API. They are different encodings — convert between them when moving between the EVM and native interfaces.
{% endhint %}

## Add Network to Your Wallet

Zilliqa is not pre-configured in MetaMask by default, so it must be added manually — follow the steps below:

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

Enter the values from the [Network Details](deploy-a-smart-contract-on-zilliqa.md#network-details) table above.
{% endstep %}

{% step %}
### Confirm the Chain ID

Confirm the Chain ID resolves to `32769`.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added Zilliqa network.
{% endstep %}
{% endstepper %}

## Funding Your Deployer

Zilliqa gas is paid in ZIL.

* **Mainnet** — acquire ZIL from an exchange and send it to your deployer's `0x…` (EVM) address.

## Deploy with Foundry

{% stepper %}
{% step %}
### Install Foundry and initialize

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
forge init hello-zilliqa && cd hello-zilliqa
```
{% endstep %}

{% step %}
### Write the contract

{% code title="src/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, Zilliqa";

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

Zilliqa's explorer is Blockscout-based, so verification uses the Blockscout verifier:

```bash
forge verify-contract <DEPLOYED_ADDRESS> src/Hello.sol:Hello \
  --chain-id 32769 \
  --verifier blockscout \
  --verifier-url https://zilliqa.blockscout.com/api
```
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create the project

```bash
mkdir hello-zilliqa && cd hello-zilliqa
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

const CHAIN_ID = 32769;

module.exports = {
  solidity: '0.8.24',
  networks: {
    zilliqa: {
      url: process.env.RPC_URL,
      chainId: CHAIN_ID,
      accounts: [process.env.PRIVATE_KEY]
    }
  },
  etherscan: {
    apiKey: { zilliqa: 'empty' },
    customChains: [
      {
        network: 'zilliqa',
        chainId: CHAIN_ID,
        urls: {
          apiURL: 'https://zilliqa.blockscout.com/api',
          browserURL: 'https://zilliqa.blockscout.com'
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
    string public greeting = "Hello, Zilliqa";

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

npx hardhat ignition deploy ./ignition/modules/Hello.js --network zilliqa
```
{% endstep %}

{% step %}
### Verify on the block explorer

```bash
npx hardhat verify --network zilliqa <DEPLOYED_ADDRESS>
```
{% endstep %}
{% endstepper %}

## After Deploying

* Read contract state through the `eth_call` method against your GetBlock endpoint.
* Watch contract events with `eth_getLogs`.
* Explore your contract and transactions on the [Zilliqa explorer](https://zilliqa.blockscout.com/).
