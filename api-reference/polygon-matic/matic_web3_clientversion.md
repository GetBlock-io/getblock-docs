---
description: >-
  Learn how to use web3_clientVersion JSON-RPC method to retrieve the Polygon client
  version. Complete guide with examples for identifying node software and version info.
---

# web3\_clientVersion - Polygon

{% hint style="info" %}
The `web3_clientVersion` method returns the current client version of the Polygon node. This method is useful for identifying the specific implementation and version of the blockchain client software running on the node.
{% endhint %}

### Use Cases

* **Node Verification**: Confirm you're connected to the correct Polygon node implementation (Bor)
* **Compatibility Checks**: Verify the node version supports specific features or RPC methods
* **Debugging**: Identify potential version-specific issues or behaviors
* **Monitoring**: Track node software versions across your infrastructure

### Parameters

None

### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
  "jsonrpc": "2.0",
  "method": "web3_clientVersion",
  "params": [],
  "id": "getblock.io"
}'
```
{% endtab %}
{% endtabs %}

#### Response

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "bor/v0.3.9-stable/linux-amd64/go1.19.1"
}
```
