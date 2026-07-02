---
description: This contain all the methods for flashblock on Base network
---

# Flashblocks API

This section contains all the flashblock methods available on the Base network

### Quickstart

In this section, you will learn how to fetch the latest Flashblock — the current in-progress block containing every preconfirmed transaction — using either:

* **Axios** (JavaScript / Node.js)
* **Python** (Requests library)

Before you begin, you must have already installed [`npm`](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm) or [`yarn`](https://classic.yarnpkg.com/lang/en/docs/install) on your local machine (for the Axios example) or Python and pip (for the Python example).

{% tabs %}
{% tab title="JavaScript (Axios)" %}
{% stepper %}
{% step %}
### Setup project

```bash
mkdir flashblocks-quickstart
cd flashblocks-quickstart
npm init --yes
```
{% endstep %}

{% step %}
### Install Axios

```bash
npm install axios
```
{% endstep %}

{% step %}
### Create file

Create a new file named `index.js`. This is where you will make your first call.
{% endstep %}

{% step %}
### Set ES module type

Set the ES module `"type": "module"` in your `package.json`.
{% endstep %}

{% step %}
### Add code

{% code title="index.js" %}
```javascript
import axios from 'axios';

// Fetch the latest Flashblock (in-progress block with preconfirmed transactions)
const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_getBlockByNumber",
    "params": ["pending", false],
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers: {
        'Content-Type': 'application/json'
    },
    data: data
};

axios(config)
    .then(response => console.log(JSON.stringify(response.data, null, 2)))
    .catch(error => console.log(error));
```
{% endcode %}

Replace `<ACCESS-TOKEN>` with your actual access token from GetBlock (Base or Optimism endpoint).
{% endstep %}

{% step %}
### Run the script

```bash
node index.js
```

The response contains the current in-progress block, updated every 200ms (Base) or 250ms (Optimism). Call it again a few hundred milliseconds later and you'll see additional transactions included as new Flashblocks land.
{% endstep %}

{% step %}
### Result

{% code overflow="wrap" %}
```bash
{
    "jsonrpc": "2.0",
    "result": {
        "hash": "0x0000000000000000000000000000000000000000000000000000000000000000",
        "parentHash": "0x64c3e43c9701407d6fb8bd853782a7fc77ba7f63e55a1cd1c5eaa1c0eaf3e8c4",
        "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
        "miner": "0x4200000000000000000000000000000000000011",
        "stateRoot": "0x0000000000000000000000000000000000000000000000000000000000000000",
        "transactionsRoot": "0x38131951ed5e3c8fa20b3cb08a75ccc33cb19e3a3cdf6260eae76a173900b934",
        "receiptsRoot": "0x83434863672598b03e64b1effdb3ad205345ad5114fd8b55995ad26571663a40",
        "logsBloom": "0x363031c23351743880d55105b76437b4e05246402bc44898a3353f4531021a41092f44aab543e301cf9f41d3a1a636817fd88880854d2114e7260fc360b46ef01f7a875144df34dc8f308f08ab498b24167c560d99ff32b5b0d3676a87638c373f9ea87cbe7a0b186609584f972b2ca8b1608b028894556464c28195591b539094351a2d3b3979be4623f816c522b87e372b826b2aa47a4cf27f20759540d4990e742b86a08811cf5e40b7370dcec2807bc44273421c8e5afff70b23407a5cd2be20996a72898ce94611dcd1ab2590a5080d878a8e78f83af193c6c3f908ec64621ebd8c179c90a3b4420f2c0aeb5cb6f0ba86c2b59e2956b2343d70af419883",
        "difficulty": "0x0",
        "number": "0x2de03b4",
        "gasLimit": "0x17d78400",
        "gasUsed": "0x11b1f57",
        "timestamp": "0x6a46644b",
        "extraData": "0x01000000640000000500000000004c4b40",
        "mixHash": "0xcb2806e22a6932ee882e44cb4899082c4eb130745740b14f5110d32c0e463236",
        "nonce": "0x0000000000000000",
        "baseFeePerGas": "0x4c4b40",
        "withdrawalsRoot": "0x5a6218671840987892338b76625664c7060d7b55b9e82414f9117f5388c406d4",
        "blobGasUsed": "0x351e14",
        "excessBlobGas": "0x0",
        "parentBeaconBlockRoot": "0x41922b97f16dd4c26d9680c0888d6eca48a96638988a310578c91cb1f76865ea",
        "requestsHash": "0xe3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855",
        "uncles": [],
        "transactions": [
            "0x736d0f7b161888aef73271d4d147fa4ba5a4a42b1ef018fdab3605091aee21e3"      "0xe58be418f85a1281a3c0ba4951c33edf948568879acda466663d568512ec8e92",        "0x5bf2fba339a689973db07c54e1c22556c07a0c7fdcdf3f4918b2b7bd6fee75ab"     "0xa6bec80fa9c94c0d330e27421e414eaef85cc39800120a557b3e93dcb9e81adb",
            "0x7f291369918ece59ccc88efde10a9807aa7da285dfec7744d5cc184b75227ba8"
        ],
        "withdrawals": []
    },
    "id": 1
}

```
{% endcode %}
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Python (Requests)" %}
{% stepper %}
{% step %}
### Setup the project directory

```bash
mkdir flashblocks-quickstart
cd flashblocks-quickstart
```
{% endstep %}

{% step %}
### Create and activate a virtual environment

```bash
python -m venv venv
source venv/bin/activate

# On Windows, use venv\Scripts\activate
```
{% endstep %}

{% step %}
### Install requests

```bash
pip install requests
```
{% endstep %}

{% step %}
### Create script

{% code title="main.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_getBlockByNumber",
    "params": ["pending", False],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endcode %}

Replace `<ACCESS-TOKEN>` with your actual access token from GetBlock.
{% endstep %}

{% step %}
### Run the script

```bash
python main.py
```
{% endstep %}
{% endstepper %}
{% endtab %}
{% endtabs %}
