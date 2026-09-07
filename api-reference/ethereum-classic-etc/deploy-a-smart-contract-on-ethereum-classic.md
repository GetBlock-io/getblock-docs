# Deploy a Smart Contract on Ethereum Classic

Ethereum Classic is EVM-compatible, so contracts deploy with the standard Solidity toolchain — Foundry, Hardhat, or Remix — pointed at a GetBlock endpoint. This guide covers adding the network to a wallet and deploying a first contract to Ethereum Classic.

## Prerequisites

* [Node.js](https://nodejs.org/) 18+ and a package manager, or [Foundry](https://book.getfoundry.sh/)
* An EVM wallet (such as MetaMask) with ETC on Ethereum Classic for gas — see [Add Network to Your Wallet](deploy-a-smart-contract-on-ethereum-classic.md#add-network-to-your-wallet) and [Funding Your Deployer](deploy-a-smart-contract-on-ethereum-classic.md#funding-your-deployer)
* A GetBlock access token — the endpoint is `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`
* A funded deployer address with ETC on Ethereum Classic

## Network Details

| Property        | Value                                             |
| --------------- | ------------------------------------------------- |
| Network Name    | Ethereum Classic                                  |
| RPC URL         | https://shared.eu-central-1.getblock.io//         |
| Chain ID        | 61 (0x3d)                                         |
| Currency Symbol | ETC                                               |
| Block Explorer  | [etc.blockscout.com](https://etc.blockscout.com/) |

## Add Network to Your Wallet

Ethereum Mainnet is pre-configured in MetaMask by default. Ethereum Classic is not, so it must be added manually — use the button below or the manual steps that follow.

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

Enter the values from the [Network Details](deploy-a-smart-contract-on-ethereum-classic.md#network-details) table above.
{% endstep %}

{% step %}
### Confirm the Chain ID

Confirm the Chain ID resolves to `61`.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added Ethereum Classic network.
{% endstep %}
{% endstepper %}

## Funding Your Deployer

Ethereum Classic gas is paid in ETC.

* **Mainnet** — acquire ETC from an exchange, then send it to your deployer's address.
* **Testnet (Mordor, chain ID 63)** — request test ETC from a Mordor faucet; see [ethereumclassic.org](https://ethereumclassic.org/) and the Mordor testnet resources for the current faucet and endpoints.

## Deploy with Foundry

{% stepper %}
{% step %}
### Install Foundry and initialize

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
forge init hello-etc && cd hello-etc
```
{% endstep %}

{% step %}
### Write the contract

{% code title="src/Hello.sol" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Hello {
    string public greeting = "Hello, Ethereum Classic";

    function setGreeting(string calldata greeting_) external {
        greeting = greeting_;
    }
}
```
{% endcode %}
{% endstep %}

{% step %}
### Deploy

Because Ethereum Classic uses legacy gas (no EIP-1559), send a legacy transaction:

```bash
export ETC_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

forge create src/Hello.sol:Hello \
  --rpc-url $ETC_RPC_URL \
  --private-key $PRIVATE_KEY \
  --legacy \
  --broadcast
```
{% endstep %}

{% step %}
### Verify the source

Ethereum Classic's explorer verifies contracts through Sourcify:

```bash
forge verify-contract <DEPLOYED_ADDRESS> src/Hello.sol:Hello \
  --chain-id 61 \
  --verifier sourcify
```
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create the project

```bash
mkdir hello-etc && cd hello-etc
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

const ETC_CHAIN_ID = 61;

module.exports = {
  solidity: '0.8.24',
  networks: {
    etc: {
      url: process.env.ETC_RPC_URL,
      chainId: ETC_CHAIN_ID,
      gasPrice: 'auto',
      accounts: [process.env.PRIVATE_KEY]
    }
  },
  sourcify: {
    enabled: true
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
    string public greeting = "Hello, Ethereum Classic";

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
export ETC_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
export PRIVATE_KEY=0xyour_deployer_private_key

npx hardhat ignition deploy ./ignition/modules/Hello.js --network etc
```
{% endstep %}

{% step %}
### Verify the source

```bash
npx hardhat verify --network etc <DEPLOYED_ADDRESS>
```
{% endstep %}
{% endstepper %}

{% hint style="info" %}
Ethereum Classic does not implement EIP-1559. Configure your tooling to send legacy (`gasPrice`) transactions rather than EIP-1559 (`maxFeePerGas` / `maxPriorityFeePerGas`) transactions, or broadcasts may be rejected.
{% endhint %}
