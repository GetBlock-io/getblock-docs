---
description: >-
  Example code for the EstimateEnergy gRPC method. Complete guide on how to use
  EstimateEnergy gRPC method in GetBlock Web3 documentation.
---

# EstimateEnergy - Tron

This method estimates the Energy required to execute a smart-contract call, without submitting a transaction. It is used to set a fee limit before a call.

## Service

This method is served by the `protocol.Wallet` service.

## Method

```protobuf
rpc EstimateEnergy (TriggerSmartContract) returns (EstimateEnergyMessage)
```

## Request Message

`TriggerSmartContract`

| Field             | Type  | Description                                         |
| ----------------- | ----- | --------------------------------------------------- |
| owner\_address    | bytes | Caller address as raw bytes                         |
| contract\_address | bytes | Contract address as raw bytes                       |
| data              | bytes | ABI-encoded call data (selector + arguments)        |
| call\_value       | int64 | TRX in SUN to send with the call                    |
| fee\_limit        | int64 | Maximum TRX in SUN to spend on Energy (write calls) |

## Request

{% tabs %}
{% tab title="grpcurl" %}
{% code overflow="wrap" %}
```bash
grpcurl -H 'x-api-key: <ACCESS-TOKEN>' \
  -d '{"owner_address": "41f0cc5a2a84cd0f68ed1667070934542d673acbd8", "contract_address": "41a614f803b6fd780986a42c78ec9c7f77e6ded13c", "data": "a9059cbb..."}' \
  shared.eu-central-1.getblock.io:443 protocol.Wallet/EstimateEnergy
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import grpc
# Stubs generated from java-tron's api.proto with grpcio-tools
from api import api_pb2_grpc
from core import Contract_pb2, Tron_pb2

creds = grpc.ssl_channel_credentials()
channel = grpc.secure_channel('shared.eu-central-1.getblock.io:443', creds)
metadata = [('x-api-key', '<ACCESS-TOKEN>')]
stub = api_pb2_grpc.WalletStub(channel)

response = stub.EstimateEnergy(Contract_pb2.TriggerSmartContract(
    owner_address=bytes.fromhex('41f0cc5a2a84cd0f68ed1667070934542d673acbd8'),
    contract_address=bytes.fromhex('41a614f803b6fd780986a42c78ec9c7f77e6ded13c'),
    data=bytes.fromhex('a9059cbb...')), metadata=metadata)
print(response)
```
{% endcode %}
{% endtab %}

{% tab title="Go" %}
{% code title="example.go" %}
```go
conn, _ := grpc.Dial(
    "shared.eu-central-1.getblock.io:443",
    grpc.WithTransportCredentials(credentials.NewTLS(&tls.Config{})),
)
defer conn.Close()

client := api.NewWalletClient(conn)
ctx := metadata.AppendToOutgoingContext(
    context.Background(), "x-api-key", "<ACCESS-TOKEN>")

resp, _ := client.EstimateEnergy(ctx, &core.TriggerSmartContract{OwnerAddress: owner, ContractAddress: contract, Data: callData})
fmt.Println(resp)
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Message

`EstimateEnergyMessage`

| Field            | Type   | Description                            |
| ---------------- | ------ | -------------------------------------- |
| result           | Return | Whether the estimate succeeded         |
| energy\_required | int64  | Estimated Energy the call will consume |

## Use Cases

* **Fee Limits**: Set fee\_limit from the estimated Energy
* **Cost Preview**: Show a user the Energy cost of an action
* **Preflight**: Detect a call that would fail before submitting
* **Batch Sizing**: Sum estimates across a batch of calls

## Error Handling

| Code | Status            | Description                                                 |
| ---- | ----------------- | ----------------------------------------------------------- |
| 3    | INVALID\_ARGUMENT | A request field is missing or malformed                     |
| 5    | NOT\_FOUND        | The requested account, block, or transaction does not exist |
| 16   | UNAUTHENTICATED   | The access token is missing or invalid                      |
| 13   | INTERNAL          | The node failed to process the request                      |
