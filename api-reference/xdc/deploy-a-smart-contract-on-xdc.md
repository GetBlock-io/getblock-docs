---
description: Learn how to deploy a smart contract on XDC using GetBlock's RPC endpoint.
---

# Deploy a Smart Contract on XDC

XDC is EVM-compatible, so contracts deploy with the standard Solidity toolchain — Foundry, Hardhat, or Remix — pointed at a GetBlock endpoint, with no chain-specific compiler or plugin required. Gas is paid in XDC. This guide covers adding the network to a wallet and deploying a first contract.

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager, or [Foundry](https://book.getfoundry.sh/)
* An EVM wallet (such as MetaMask) with XDC for gas — see [Add Network to Your Wallet](deploy-a-smart-contract-on-xdc.md#add-network-to-your-wallet) and [Funding Your Deployer](deploy-a-smart-contract-on-xdc.md#funding-your-deployer)
* A GetBlock access token — the endpoint is `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with XDC

## Network Details

| Property        | Value                                     |
| --------------- | ----------------------------------------- |
| Network Name    | XDC Network                               |
| RPC URL         | https://shared.eu-central-1.getblock.io// |
| Chain ID        | 50 (0x32)                                 |
| Currency Symbol | XDC                                       |
| Block Explorer  | [xdcscan.io](https://xdcscan.io/)         |

{% hint style="info" %}
XDC addresses are often shown with an `xdc` prefix (e.g. `xdc1234…`) instead of `0x`. They are the same bytes — the `eth_*` JSON-RPC methods here use the standard `0x` form. Convert `xdc…` to `0x…` before passing an address to an RPC call.
{% endhint %}

## Add Network to Your Wallet

XDC is not pre-configured in MetaMask by default, so it must be added manually — follow the steps below:

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

Enter the values from the [Network Details](deploy-a-smart-contract-on-xdc.md#network-details) table above.
{% endstep %}

{% step %}
### Confirm the chain ID

Confirm the Chain ID resolves to `50`.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added XDC network.
{% endstep %}
{% endstepper %}

## Funding Your Deployer

XDC gas is paid in XDC.

* **Mainnet** — acquire XDC from an exchange, then send it to your deployer's address.

## Deploy with Foundry

{% stepper %}
{% step %}
### Install Foundry and initialize

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
forge init hello-xdc && cd hello-xdc
```
{% endstep %}

{% step %}
### Write the contract

{% code title="src/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, XDC";

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

XDC's explorer is Etherscan-family, so verification uses the Etherscan verifier with an API key (set it as an environment variable):

```bash
forge verify-contract <DEPLOYED_ADDRESS> src/Hello.sol:Hello \
  --chain-id 50 \
  --verifier etherscan \
  --verifier-url https://api.xdcscan.io/api \
  --etherscan-api-key $XDCSCAN_API_KEY
```
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create the project

```bash
mkdir hello-xdc && cd hello-xdc
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

const CHAIN_ID = 50;

module.exports = {
  solidity: '0.8.24',
  networks: {
    xdc: {
      url: process.env.RPC_URL,
      chainId: CHAIN_ID,
      accounts: [process.env.PRIVATE_KEY]
    }
  },
  etherscan: {
    apiKey: { xdc: process.env.XDCSCAN_API_KEY },
    customChains: [
      {
        network: 'xdc',
        chainId: CHAIN_ID,
        urls: {
          apiURL: 'https://api.xdcscan.io/api',
          browserURL: 'https://xdcscan.io'
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
    string public greeting = "Hello, XDC";

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

npx hardhat ignition deploy ./ignition/modules/Hello.js --network xdc
```
{% endstep %}

{% step %}
### Verify on the block explorer

```bash
npx hardhat verify --network xdc <DEPLOYED_ADDRESS>
```
{% endstep %}
{% endstepper %}

## After Deploying

* Read contract state through the [eth\_call](/broken/pages/8289e29978bbb3aa1799cb05a5be3d156d400be9) method against your GetBlock endpoint
* Watch contract events with [eth\_getLogs](/broken/pages/23e319aadbcbf1d967b40bc5eff04a2d5694c276)
* Explore your contract and transactions on the [XDC explorer](https://xdcscan.io/)
