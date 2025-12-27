---
title: const { ethers } = require(...
---

{% tabs %}
{% tab title="Ether.js" %}
{% code overflow="wrap" %}
```javascript
import  ethers from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io//');

async function getCode() {
    const code = await provider.getCode('0xContractAddress');
    console.log('Contract Code Length:', code.length);
    console.log('Is Contract:', code !== '0x');
}

getCode();

```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
```jsx
import { createPublicClient, http } from "viem";
import { hyperEvm } from "viem/chains";

// Create Viem client with GetBlock
const client = createPublicClient({
  chain: hyperEvm,
  transport: http(import.meta.env.HYPER_ACCESS_TOKEN),
});

// Using the method through Viem
async function Call() {
  try {
    // Method-specific Viem implementation
    const result = await client.request({
    "method": "eth_getBlockByNumber",
    "params": ["latest", true],
};
    return result;
  } catch (error) {
    console.error("Viem Error:", error);
    throw error;
  }
}
```
{% endtab %}
{% endtabs %}
