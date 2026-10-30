# Deploy Smart Contract on Hashkey

HashKey Chain is EVM-compatible, so contracts deploy with the standard Solidity toolchain — Foundry, Hardhat, or Remix — pointed at a GetBlock endpoint, with no chain-specific compiler or plugin required. Gas is paid in HSK. This guide covers adding the network to a wallet and deploying a first contract.

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager, or [Foundry](https://book.getfoundry.sh/)
* An EVM wallet (such as MetaMask) with HSK for gas — see [Add Network to Your Wallet](deploy-smart-contract-on-hashkey.md#add-network-to-your-wallet) and [Funding Your Deployer](deploy-smart-contract-on-hashkey.md#funding-your-deployer)
* A GetBlock access token — the endpoint is `https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with HSK

## Network Details

| Property        | Value                                                     |
| --------------- | --------------------------------------------------------- |
| Network Name    | HashKey Chain Mainnet                                     |
| RPC URL         | https://shared.us-east-1.getblock.io//                    |
| Chain ID        | 177 (0xb1)                                                |
| Currency Symbol | HSK                                                       |
| Block Explorer  | [hashkey.blockscout.com](https://hashkey.blockscout.com/) |

## Add Network to Your Wallet

HashKey Chain is not pre-configured in MetaMask by default, so it must be added manually — follow the steps below:

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

Enter the values from the [Network Details](deploy-smart-contract-on-hashkey.md#network-details) table above.
{% endstep %}

{% step %}
### Confirm the Chain ID

Confirm the Chain ID resolves to `177`.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added HashKey Chain network.
{% endstep %}
{% endstepper %}

## Funding Your Deployer

HashKey Chain gas is paid in HSK.

* **Mainnet** — acquire HSK from an exchange or bridge to HashKey Chain, then send it to your deployer's address.

## Deploy with Foundry

{% stepper %}
{% step %}
### Install Foundry and initialize

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
forge init hello-hashkey && cd hello-hashkey
```
{% endstep %}

{% step %}
### Write the contract

{% code title="src/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, HashKey Chain";

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
export RPC_URL=https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

forge create src/Hello.sol:Hello \
  --rpc-url $RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```
{% endstep %}

{% step %}
### Verify on the block explorer

HashKey Chain's explorer is Blockscout-based, so verification uses the Blockscout verifier:

```bash
forge verify-contract <DEPLOYED_ADDRESS> src/Hello.sol:Hello \
  --chain-id 177 \
  --verifier blockscout \
  --verifier-url https://hashkey.blockscout.com/api/
```
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create the project

```bash
mkdir hello-hashkey && cd hello-hashkey
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

const CHAIN_ID = 177;

module.exports = {
  solidity: '0.8.24',
  networks: {
    hashkey: {
      url: process.env.RPC_URL,
      chainId: CHAIN_ID,
      accounts: [process.env.PRIVATE_KEY]
    }
  },
  etherscan: {
    apiKey: { hashkey: 'empty' },
    customChains: [
      {
        network: 'hashkey',
        chainId: CHAIN_ID,
        urls: {
          apiURL: 'https://hashkey.blockscout.com/api',
          browserURL: 'https://hashkey.blockscout.com'
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
    string public greeting = "Hello, HashKey Chain";

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
export RPC_URL=https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

npx hardhat ignition deploy ./ignition/modules/Hello.js --network hashkey
```
{% endstep %}

{% step %}
### Verify on the block explorer

```bash
npx hardhat verify --network hashkey <DEPLOYED_ADDRESS>
```
{% endstep %}
{% endstepper %}

## After Deploying

* Read contract state through the `eth_call` method against your GetBlock endpoint.
* Watch contract events with `eth_getLogs`.
* Explore your contract and transactions on the [HashKey explorer](https://hashkey.blockscout.com/).
