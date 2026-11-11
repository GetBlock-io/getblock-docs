---
description: >-
  Example code for the BroadcastTransaction gRPC method. Complete guide on how
  to use BroadcastTransaction gRPC method in GetBlock Web3 documentation.
---

# BroadcastTransaction - Tron

This method submits a signed transaction to the network. It accepts the full signed Transaction message and returns whether it was accepted.

## Service

This method is served by the `protocol.Wallet` service.

## Method

{% code overflow="wrap" %}
```protobuf
rpc BroadcastTransaction (Transaction) returns (Return)
```
{% endcode %}

## Request Message

`Transaction`

| Field     | Type           | Description                                   |
| --------- | -------------- | --------------------------------------------- |
| raw\_data | raw            | The transaction body, including its contracts |
| signature | repeated bytes | Signatures over the transaction               |

## Request

{% tabs %}
{% tab title="grpcurl" %}
{% code overflow="wrap" %}
```bash
grpcurl -H 'x-api-key: <ACCESS-TOKEN>' \
  -d '{"raw_data": {"contract": []}, "signature": ["<signature-bytes>"]}' \
  shared.eu-central-1.getblock.io:443 protocol.Wallet/BroadcastTransaction
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

response = stub.BroadcastTransaction(signed_transaction, metadata=metadata)
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

resp, _ := client.BroadcastTransaction(ctx, signedTx)
fmt.Println(resp)
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Message

`Return`

| Field   | Type           | Description                           |
| ------- | -------------- | ------------------------------------- |
| result  | bool           | true when the transaction is accepted |
| code    | response\_code | Rejection code, when not accepted     |
| message | bytes          | Rejection reason, when not accepted   |

## Use Cases

* **Payment Broadcast**: Submit a signed TRX or token transfer
* **Contract Calls**: Broadcast a signed contract call
* **dApp Backends**: Relay transactions signed on the client
* **Retry Flows**: Resubmit a signed transaction from stored data

## Error Handling

| Code | Status            | Description                                                 |
| ---- | ----------------- | ----------------------------------------------------------- |
| 3    | INVALID\_ARGUMENT | A request field is missing or malformed                     |
| 5    | NOT\_FOUND        | The requested account, block, or transaction does not exist |
| 16   | UNAUTHENTICATED   | The access token is missing or invalid                      |
| 13   | INTERNAL          | The node failed to process the request                      |
