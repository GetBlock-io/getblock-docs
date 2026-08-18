---
description: >-
  This guide covers adding the network to a browser wallet, then deploying a
  simple contract using Foundry and Hardhat — pick whichever you prefer.
---

# Deploy smart contract on Ronin

Ronin is EVM-compatible, so contracts deploy with standard Ethereum tooling such as Foundry, Hardhat, and Remix. This guide covers adding Ronin to a wallet and deploying a first contract with Foundry and Hardhat through a GetBlock endpoint. Ronin verifies contracts through Sourcify rather than an Etherscan-style explorer.

## Prerequisites

* A wallet holding RON for gas (see Add Network to Your Wallet below)
* A GetBlock access token from the GetBlock dashboard, used as `<ACCESS-TOKEN>` in the endpoint `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`
* Node.js version 20 or later, for Hardhat
* A funded deployer address, on Saigon Testnet for testing (see Testnet Faucets)

### Network Details

| Property        | Ronin Mainnet                                                      | Saigon Testnet                                                                   |
| --------------- | ------------------------------------------------------------------ | -------------------------------------------------------------------------------- |
| Chain ID        | 2020 (0x7e4)                                                       | 202601 (0x31769)                                                                 |
| RPC URL         | `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`                           | `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`                                         |
| Currency Symbol | RON                                                                | RON                                                                              |
| Block Explorer  | [app.roninchain.com/explorer](https://app.roninchain.com/explorer) | [saigon-app.roninchain.com/explorer](https://saigon-app.roninchain.com/explorer) |

{% hint style="info" %}
Ronin is not pre-configured in MetaMask by default, and is best used with the Ronin Wallet browser extension. To use MetaMask, add Ronin manually or through an add-network button before it appears in the wallet.
{% endhint %}

{% hint style="warning" %}
Every command below defaults to Mainnet — replace environment variable values with testnet ones for a dry run before touching real funds.
{% endhint %}

{% hint style="warning" %}
Never commit a real private key. Use environment variables and prefer a throwaway deployer key for testing. Private keys committed to a public repo are drained within seconds by automated bots.
{% endhint %}

## Add Network to Your Wallet

{% stepper %}
{% step %}
### Open your wallet

Open the Ronin Wallet extension, or MetaMask.
{% endstep %}

{% step %}
### Open the network menu

Click the network dropdown at the top of the wallet.
{% endstep %}

{% step %}
### Add a network manually

Click **Add network**, then **Add a network manually**.
{% endstep %}

{% step %}
### Enter the network details

Enter the network details from the Prerequisites section above.
{% endstep %}

{% step %}
### Save and switch networks

Save and switch to the newly added network.
{% endstep %}
{% endstepper %}

## Deploy with Foundry

{% stepper %}
{% step %}
### Install Foundry

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
```
{% endstep %}

{% step %}
### Create a project

```bash
mkdir ronin-deploy && cd ronin-deploy
forge init
```
{% endstep %}

{% step %}
### Create a contract

Create `src/HelloRonin.sol`:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.28;

contract HelloRonin {
    function hello() external pure returns (string memory) {
        return "Hello from Ronin";
    }
}
```
{% endstep %}

{% step %}
### Deploy

```bash
export PRIVATE_KEY=<your-deployer-private-key>
export RONIN_RPC_URL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

forge create src/HelloRonin.sol:HelloRonin \
  --rpc-url $RONIN_RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```
{% endstep %}

{% step %}
### Verify with Sourcify

Ronin uses Sourcify for contract verification, not an Etherscan-style API. Verify against Ronin's Sourcify server:

```bash
forge verify-contract <contract_address> \
  src/HelloRonin.sol:HelloRonin \
  --chain-id 2020 \
  --verifier sourcify \
  --verifier-url https://sourcify.roninchain.com/server
```

Then open the contract in the Ronin explorer and confirm the Contracts tab shows a verified checkmark.
{% endstep %}
{% endstepper %}

## Deploy with Hardhat

{% stepper %}
{% step %}
### Create a project

```bash
mkdir ronin-deploy && cd ronin-deploy
npm init -y
npm install --save-dev hardhat
npx hardhat init
```

Select a JavaScript project when prompted.
{% endstep %}

{% step %}
### Configure networks

{% code title="hardhat.config.js" %}
```javascript
require('@nomicfoundation/hardhat-toolbox');

const PRIVATE_KEY = process.env.PRIVATE_KEY;

module.exports = {
  solidity: '0.8.28',
  networks: {
    ronin: {
      url: 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
      chainId: 2020,
      accounts: [PRIVATE_KEY]
    },
    saigon: {
      url: 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
      chainId: 202601,
      accounts: [PRIVATE_KEY]
    }
  }
};
```
{% endcode %}
{% endstep %}

{% step %}
### Create a contract

Create `contracts/HelloRonin.sol`:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.28;

contract HelloRonin {
    function hello() external pure returns (string memory) {
        return "Hello from Ronin";
    }
}
```
{% endstep %}

{% step %}
### Compile and deploy

Create `scripts/deploy.js`:

{% code title="scripts/deploy.js" %}
```javascript
const hre = require('hardhat');

async function main() {
  const contract = await hre.ethers.deployContract('HelloRonin');
  await contract.waitForDeployment();
  console.log('HelloRonin deployed to:', await contract.getAddress());
}

main().catch((error) => {
  console.error(error);
  process.exitCode = 1;
});
```
{% endcode %}

Then compile and deploy:

```bash
npx hardhat compile
npx hardhat run scripts/deploy.js --network ronin
```
{% endstep %}

{% step %}
### Verify with Sourcify

Publish the source to Ronin's Sourcify server:

```bash
npx hardhat --network ronin sourcify \
  --endpoint https://sourcify.roninchain.com/server
```

In the Ronin explorer, search for the contract, open the Contracts tab, and confirm the green verified checkmark.
{% endstep %}
{% endstepper %}

## Testnet Faucets

### Saigon Testnet

* [Ronin Faucet](https://faucet.roninchain.com/) — dispenses Saigon test RON to a connected wallet, roughly 0.01 RON per day (verify)
