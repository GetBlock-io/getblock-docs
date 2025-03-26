---
description: >-
  Example code for the getmempoolancestors json-rpc method. Сomplete guide on
  how to use getmempoolancestors json-rpc in GetBlock.io Web3 documentation.
---

# getmempoolancestors - Bitcoin

#### Parameters

`txid` - string, required

The transaction id (must be in mempool)

`verbose` - boolean, optional, default=false

True for a json object, false for array of transaction ids

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getmempoolancestors",
"params": ["a4aef4dd6721ff3e39a6f9b55d87ec2b72bc5bb55ea806ba75aa6c27b2a335df", null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": [
        "09338b612e0dbe246dc5f60b30e83029fd2d4ce84272afcce902215c9832de1f",
        "133e7651074cc4559bcab749c389fcfe6de9c41179a6a4efdb2ddde31be80023",
        "12018e5f38a691d567e579d0a89be6fce59ebdcbc05c96c90eff3c3fae5bf225",
        "bca8ecd71153a9857aba8cefe57272dd8f6d8a166bbca23ff9458e7df3b43127",
        "67f267952ace1b1dbd2ffe1c5b0b130590fe9c46dba02dfadf03c94b6c5a1529",
        "eee4156d3570fc09feb5596edd774a980a546a4dc9635862785e8e4446c99738",
        "7564fd2ec022a111828b1c4d04ef43eb5e6440ea2049b2656d9195d8842d804f",
        "099130b4f9a78a5df742b1d8112b565777dbe6e9bf62b378d23440e5d8268357",
        "b04546811058667e2ddd0c7660456fde219450e6faf259c1bd2331759bd9f76e",
        "bbfbafdb20ccf7f3b231a0c26441a80d5ff0fc98c7cfcd77eb7a2e04e6c48a70",
        "269889b35326364142bd506876aa23b236d18ac4cf67679c0ecabc62ca5a9e91",
        "bfa5930dd20de26f289600fc04fc9c6253a6f565a1cea579c56b28e5aeeb6399",
        "d8047111681bcb24ade1355458bee569dfa2eda33ddf0d6a3b1eaa21fca0729b",
        "c7229dcd13432d3dafb8408eb4c61c6973b585b4232039ff2a777057cd3f419f",
        "ebfa35ff32bacd2e2f31fa09b77a5baf0dd04ea69b49731c785fa14cb4ae26c6",
        "177b36ad581a51d9eaaa23d6933ee932a1c4997d12469b9ed6ea3ec83ae626c8",
        "ca603f623a08bd0e825792de6ee158d3a92af4ba512cc9202dd17af6be35dcd3",
        "65ac887d001640b4a1e490f315af37f02a8962105f0e364429879d55e967c1e2",
        "f20c3ba1ee5dd284fa75758ff3bdd2805323f6dd590f5ffe27892c672f0f8ff3",
        "7b28bf17e2757475dc5ff3709dae1d70fc1624eecc0118f5023c49c2b8db08f7"
    ]
}
```
