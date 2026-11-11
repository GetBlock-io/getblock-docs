---
description: Learn how to deploy a smart contract on TAC with GetBlock.
---

# Deploy a Smart Contract on TAC

TAC's execution layer is EVM-compatible (cosmos/evm), so contracts deploy with the standard Solidity toolchain — Foundry, Hardhat, or Remix — pointed at a GetBlock endpoint, with no chain-specific compiler or plugin required. Gas is paid in TAC. This guide covers adding the network to a wallet and deploying a first contract to TAC.

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager, or [Foundry](https://book.getfoundry.sh/)
* An EVM wallet (such as MetaMask) with TAC for gas — see [Add Network to Your Wallet](deploy-a-smart-contract-on-tac.md#add-network-to-your-wallet) and [Funding Your Deployer](deploy-a-smart-contract-on-tac.md#funding-your-deployer)
* A GetBlock access token — the EVM endpoint is `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with TAC

## Network Details

| Property        | Value                                                    |
| --------------- | -------------------------------------------------------- |
| Network Name    | TAC Mainnet                                              |
| RPC URL         | https://shared.eu-central-1.getblock.io/\<ACCESS-TOKEN>/ |
| Chain ID        | 239 (0xef)                                               |
| Currency Symbol | TAC                                                      |
| Block Explorer  | [explorer.tac.build](https://explorer.tac.build/)        |

## Add Network to Your Wallet

TAC is not pre-configured in MetaMask by default, so it must be added manually — follow the steps below:

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

Enter the values from the [Network Details](deploy-a-smart-contract-on-tac.md#network-details) table above.
{% endstep %}

{% step %}
### Confirm the Chain ID

Confirm the Chain ID resolves to `239`.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added TAC network.
{% endstep %}
{% endstepper %}

{% hint style="info" %}
To target the **Saint Petersburg (SPB) testnet** instead, use chain ID `2391` (`0x957`), the SPB EVM endpoint, and the explorer at [spb.explorer.tac.build](https://spb.explorer.tac.build/).
{% endhint %}

## Funding Your Deployer

TAC gas is paid in TAC.

* **Mainnet** — acquire TAC from an exchange or bridge, then send it to your deployer's address.
* **Testnet (Saint Petersburg, chain ID 2391)** — claim test TAC from the TAC testnet faucet; see [docs.tac.build](https://docs.tac.build/) for the current faucet and endpoints.

## Deploy with Foundry

{% stepper %}
{% step %}
### Install Foundry and initialize

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
forge init hello-tac && cd hello-tac
```
{% endstep %}

{% step %}
### Write the contract

{% code title="src/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, TAC";

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
export TAC_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

forge create src/Hello.sol:Hello \
  --rpc-url $TAC_RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```
{% endstep %}

{% step %}
### Verify on the block explorer

TAC's explorer is Blockscout-based, so verification uses the Blockscout verifier:

```bash
forge verify-contract <DEPLOYED_ADDRESS> src/Hello.sol:Hello \
  --chain-id 239 \
  --verifier blockscout \
  --verifier-url https://tac.blockscout.com/api/
```
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create the project

```bash
mkdir hello-tac && cd hello-tac
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

const TAC_CHAIN_ID = 239;

module.exports = {
  solidity: '0.8.24',
  networks: {
    tac: {
      url: process.env.TAC_RPC_URL,
      chainId: TAC_CHAIN_ID,
      accounts: [process.env.PRIVATE_KEY]
    }
  },
  etherscan: {
    apiKey: { tac: 'empty' },
    customChains: [
      {
        network: 'tac',
        chainId: TAC_CHAIN_ID,
        urls: {
          apiURL: 'https://tac.blockscout.com/api',
          browserURL: 'https://explorer.tac.build'
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
    string public greeting = "Hello, TAC";

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
export TAC_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

npx hardhat ignition deploy ./ignition/modules/Hello.js --network tac
```
{% endstep %}

{% step %}
### Verify on the block explorer

```bash
npx hardhat verify --network tac <DEPLOYED_ADDRESS>
```
{% endstep %}
{% endstepper %}

## After Deploying

* Read contract state through the `eth_call` method against your GetBlock endpoint
* Watch contract events with `eth_getLogs`
* Explore your contract and transactions on [TAC Explorer](https://explorer.tac.build/)
