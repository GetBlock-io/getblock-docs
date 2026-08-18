---
description: >-
  This guide covers writing, compiling, and deploying a smart contract to
  Bitcoin Cash using CashScript, and broadcasting contract transactions through
  a GetBlock node.
---

# Deploy Smart Contract To Bitcon Cash

Bitcoin Cash contracts are compiled to Bitcoin Cash Script and locked to a Pay-to-Script-Hash (P2SH) address; deployment means funding that address, not registering code in a global contract account.

{% hint style="info" %}
Bitcoin Cash smart contracts differ fundamentally from EVM contracts. A contract is a spending condition on a UTXO, not a stateful account. There is no persistent contract storage, no global contract account, and no Solidity, Foundry, Hardhat, or MetaMask `wallet_addEthereumChain` flow. The canonical reference for the CashScript language and SDK is maintained at cashscript.org; this page describes how to build and broadcast contracts with a GetBlock node behind the network layer.
{% endhint %}

### Prerequisites

* Node.js version 20 or later
* The CashScript compiler (`cashc`) and SDK (`cashscript`), version 0.10.0 or later, which introduces the `TransactionBuilder` API used below
* A funded Bitcoin Cash address to seed the contract and pay miner fees, on chipnet for testing (see Chipnet Faucet)
* A GetBlock access token from the GetBlock dashboard, used in the endpoint `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`

### Network Details

| Property       | Mainnet                                                            | Chipnet (testing)                                         |
| -------------- | ------------------------------------------------------------------ | --------------------------------------------------------- |
| Network name   | mainnet                                                            | chipnet                                                   |
| Address type   | p2sh32 (default)                                                   | p2sh32 (default)                                          |
| GetBlock RPC   | `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`                           | Provisioned per dashboard (verify)                        |
| Block explorer | [blockchair.com/bitcoin-cash](https://blockchair.com/bitcoin-cash) | [chipnet.imaginary.cash](https://chipnet.imaginary.cash/) |

{% hint style="warning" %}
Deploy to chipnet before mainnet. Funds sent to a contract address compiled from incorrect source or constructor arguments can become permanently unspendable, because no key can satisfy a spending condition that no function encodes.
{% endhint %}

{% hint style="warning" %}
Never commit a private key or WIF to source control. Load signing keys from environment variables, and use a throwaway key for testing. Keys committed to a public repository are swept by automated bots within seconds.
{% endhint %}

### How to Deploy a Smart Contract

1. **Install the Toolchain**

```bash
mkdir bch-contract && cd bch-contract
npm init -y
npm install cashscript
npm install --save-dev cashc
```

Confirm the compiler version:

```bash
npx cashc --version
```

2\. **Write the Contract**

Create `TransferWithTimeout.cash`. This contract lets a recipient claim funds with their signature, and lets the sender reclaim the funds after a timeout block height. It takes three constructor arguments and exposes two spending functions.

{% code title="TransferWithTimeout.cash" %}
```solidity
pragma cashscript ^0.10.0;

contract TransferWithTimeout(pubkey sender, pubkey recipient, int timeout) {
    // The recipient can claim the funds at any time with their signature.
    function transfer(sig recipientSig) {
        require(checkSig(recipientSig, recipient));
    }

    // The sender can reclaim the funds after the timeout height passes.
    function timeout(sig senderSig) {
        require(checkSig(senderSig, sender));
        require(tx.time >= timeout);
    }
}
```
{% endcode %}

3. **Compile to an Artifact**

The `cashc` compiler transpiles the `.cash` source to Bitcoin Cash Script and writes a JSON artifact that the SDK imports.

```bash
npx cashc ./TransferWithTimeout.cash --output ./TransferWithTimeout.json
```

Inspect the compiled size and opcode count when optimizing:

```bash
npx cashc ./TransferWithTimeout.cash --size --opcount
```

4\. **Instantiate and Derive the Contract Address**

Instantiating the contract with its constructor arguments produces the P2SH address that funds are sent to. Deriving the address is the deployment step; the contract exists on-chain once its address holds a UTXO.

{% code title="deploy.js" %}
```javascript
import { Contract, ElectrumNetworkProvider } from 'cashscript';
import artifact from './TransferWithTimeout.json' with { type: 'json' };
import { senderPub, recipientPub } from './keys.js';

// Use chipnet for testing; switch to 'mainnet' for production.
const provider = new ElectrumNetworkProvider('chipnet');

// Constructor arguments: sender pubkey, recipient pubkey, timeout height.
// Integer amounts and arguments must be bigint.
const constructorArgs = [senderPub, recipientPub, 800000n];
const contract = new Contract(artifact, constructorArgs, { provider });

console.log('Contract address:', contract.address);
console.log('Contract balance:', await contract.getBalance());
```
{% endcode %}

5. **Fund the Contract**

Send BCH from any wallet to the contract address printed above. On Chipnet, use the faucet listed below. Once the funding transaction confirms, the contract holds a spendable UTXO.

Verify the balance and UTXO set:

{% code title="check.js" %}
```javascript
console.log('Balance (satoshis):', await contract.getBalance());
console.log('UTXOs:', await contract.getUtxos());
```
{% endcode %}

6\. **Connect Through GetBlock**

The CashScript SDK reaches the network through a `NetworkProvider`. The interface exposes four operations: `getUtxos`, `getBlockHeight`, `getRawTransaction`, and `sendRawTransaction`. A custom provider can route transaction reads, block height, and broadcasting through a GetBlock node, which handles the node-level operations reliably without running local infrastructure.

Address-indexed UTXO retrieval (`getUtxos`) is not available on shared GetBlock endpoints, because Bitcoin Cash full nodes do not index outputs by address and the wallet and address methods are disallowed on shared nodes. The provider below routes `getUtxos` to an `ElectrumNetworkProvider`, which connects to an indexing server, and routes the remaining three operations through GetBlock.

{% code title="getblock-provider.js" %}
```javascript
import { ElectrumNetworkProvider } from 'cashscript';

const ENDPOINT = 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/';

async function rpc(method, params) {
    const response = await fetch(ENDPOINT, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
            jsonrpc: '2.0',
            method: method,
            params: params,
            id: 'getblock.io'
        })
    });
    const { error, result } = await response.json();
    if (error) throw new Error(JSON.stringify(error));
    return result;
}

// Routes broadcast, raw-transaction reads, and block height through GetBlock;
// delegates address-indexed UTXO lookups to an Electrum indexer.
export class GetBlockProvider {
    constructor(network = 'mainnet') {
        this.network = network;
        this.indexer = new ElectrumNetworkProvider(network);
    }

    async getUtxos(address) {
        return this.indexer.getUtxos(address);
    }

    async getBlockHeight() {
        return rpc('getblockcount', []);
    }

    async getRawTransaction(txid) {
        return rpc('getrawtransaction', [txid, false, null]);
    }

    async sendRawTransaction(txHex) {
        return rpc('sendrawtransaction', [txHex, null]);
    }
}
```
{% endcode %}

Instantiate the contract with the GetBlock-backed provider:

{% code title="deploy-getblock.js" %}
```javascript
import { Contract } from 'cashscript';
import artifact from './TransferWithTimeout.json' with { type: 'json' };
import { senderPub, recipientPub } from './keys.js';
import { GetBlockProvider } from './getblock-provider.js';

const provider = new GetBlockProvider('mainnet');
const contract = new Contract(artifact, [senderPub, recipientPub, 800000n], { provider });

console.log('Contract address:', contract.address);
```
{% endcode %}

7. **Spend From the Contract**

Spending calls one of the contract functions and satisfies its `require` conditions. The `TransactionBuilder` adds the contract UTXO as an input, unlocked by the chosen function, and sends the balance to a destination address minus the miner fee. Broadcasting goes through the GetBlock-backed provider's `sendRawTransaction`.

{% code title="spend.js" %}
```javascript
import { TransactionBuilder, SignatureTemplate } from 'cashscript';
import { Contract } from 'cashscript';
import artifact from './TransferWithTimeout.json' with { type: 'json' };
import { senderPub, recipientPub, recipientKeys } from './keys.js';
import { GetBlockProvider } from './getblock-provider.js';

const provider = new GetBlockProvider('mainnet');
const contract = new Contract(artifact, [senderPub, recipientPub, 800000n], { provider });

const utxos = await contract.getUtxos();
const selected = utxos[0];

const minerFee = 1000n;
const sendAmount = selected.satoshis - minerFee;
const destination = 'bitcoincash:qpm2qsznhks23z7629mms6s4cwef74vcwvy22gdx6a';

const txDetails = await new TransactionBuilder({ provider })
    .addInput(selected, contract.unlock.transfer(new SignatureTemplate(recipientKeys)))
    .addOutput({ to: destination, amount: sendAmount })
    .send();

console.log('Broadcast transaction:', txDetails.txid);
```
{% endcode %}

## Chipnet Faucet

Fund a chipnet address before deploying to the test network.

* [chipnet.imaginary.cash faucet](https://tbch.googol.cash/) — dispenses chipnet BCH per request; requires a destination chipnet address (verify)
