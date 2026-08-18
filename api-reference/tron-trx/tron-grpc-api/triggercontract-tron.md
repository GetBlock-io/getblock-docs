---
description: >-
  Example code for the TriggerContract gRPC method. Complete guide on how to use
  TriggerContract gRPC method in GetBlock Web3 documentation.
---

# TriggerContract - Tron

This method builds an unsigned transaction that calls a state-changing smart-contract function, such as a TRC-20 transfer. The returned transaction must be signed and broadcast.

## Service

This method is served by the `protocol.Wallet` service.

## Method

```protobuf
rpc TriggerContract (TriggerSmartContract) returns (TransactionExtention)
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
  -d '{"owner_address": "41f0cc5a2a84cd0f68ed1667070934542d673acbd8", "contract_address": "41a614f803b6fd780986a42c78ec9c7f77e6ded13c", "data": "a9059cbb...", "fee_limit": 100000000}' \
  shared.eu-central-1.getblock.io:443 protocol.Wallet/TriggerContract
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

response = stub.TriggerContract(Contract_pb2.TriggerSmartContract(
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

resp, _ := client.TriggerContract(ctx, &core.TriggerSmartContract{OwnerAddress: owner, ContractAddress: contract, Data: callData})
fmt.Println(resp)
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Message

`TransactionExtention`

| Field            | Type           | Description                                         |
| ---------------- | -------------- | --------------------------------------------------- |
| transaction      | Transaction    | The built, unsigned transaction                     |
| txid             | bytes          | The transaction id                                  |
| result           | Return         | Build result, with a result flag and any error code |
| constant\_result | repeated bytes | Return data for constant (read-only) calls          |
| energy\_used     | int64          | Energy the call consumes                            |

## Use Cases

* **Token Transfers**: Build a TRC-20 transfer such as USDT
* **Contract Writes**: Call any state-changing contract function
* **dApp Actions**: Build swaps, approvals, and mints for signing
* **Fee Control**: Cap Energy spend with fee\_limit

## Error Handling

| Code | Status            | Description                                                 |
| ---- | ----------------- | ----------------------------------------------------------- |
| 3    | INVALID\_ARGUMENT | A request field is missing or malformed                     |
| 5    | NOT\_FOUND        | The requested account, block, or transaction does not exist |
| 16   | UNAUTHENTICATED   | The access token is missing or invalid                      |
| 13   | INTERNAL          | The node failed to process the request                      |
