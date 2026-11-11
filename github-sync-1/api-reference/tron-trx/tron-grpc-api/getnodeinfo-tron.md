---
description: >-
  Example code for the GetNodeInfo gRPC method. Complete guide on how to use
  GetNodeInfo gRPC method in GetBlock Web3 documentation.
---

# GetNodeInfo - Tron

This method returns diagnostic information about the node, including its version, active connections, and block-sync state.

## Service

This method is served by the `protocol.Wallet` service.

## Method

```protobuf
rpc GetNodeInfo (EmptyMessage) returns (NodeInfo)
```

## Request Message

`EmptyMessage`

This message has no fields.

## Request

{% tabs %}
{% tab title="grpcurl" %}
{% code overflow="wrap" %}
```bash
grpcurl -H 'x-api-key: <ACCESS-TOKEN>' \
  -d '{}' \
  shared.eu-central-1.getblock.io:443 protocol.Wallet/GetNodeInfo
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

response = stub.GetNodeInfo(Tron_pb2.EmptyMessage(), metadata=metadata)
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

resp, _ := client.GetNodeInfo(ctx, &api.EmptyMessage{})
fmt.Println(resp)
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Message

`NodeInfo`

| Field               | Type           | Description                                         |
| ------------------- | -------------- | --------------------------------------------------- |
| block               | string         | The node's current head block                       |
| solidityBlock       | string         | The node's latest confirmed (Solidity) block        |
| currentConnectCount | int32          | Number of peer connections                          |
| configNodeInfo      | ConfigNodeInfo | Node configuration, including the java-tron version |

## Use Cases

* **Health Checks**: Confirm a node is connected and syncing
* **Confirmation Lag**: Compare head and Solidity block heights
* **Version Reporting**: Read the java-tron version for support
* **Peer Monitoring**: Track peer connection counts

## Error Handling

| Code | Status            | Description                                                 |
| ---- | ----------------- | ----------------------------------------------------------- |
| 3    | INVALID\_ARGUMENT | A request field is missing or malformed                     |
| 5    | NOT\_FOUND        | The requested account, block, or transaction does not exist |
| 16   | UNAUTHENTICATED   | The access token is missing or invalid                      |
| 13   | INTERNAL          | The node failed to process the request                      |
