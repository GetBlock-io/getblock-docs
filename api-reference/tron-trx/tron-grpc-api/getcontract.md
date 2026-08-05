# GetContract

This method returns the metadata and ABI of a smart contract deployed at an address.

## Service

This method is served by the `protocol.Wallet` service.

## Method

```protobuf
rpc GetContract (BytesMessage) returns (SmartContract)
```

## Request Message

`BytesMessage`

| Field | Type  | Description                       |
| ----- | ----- | --------------------------------- |
| value | bytes | The contract address as raw bytes |

## Request

{% tabs %}
{% tab title="grpcurl" %}
{% code overflow="wrap" %}
```bash
grpcurl -H 'x-api-key: <ACCESS-TOKEN>' \
  -d '{"value": "41a614f803b6fd780986a42c78ec9c7f77e6ded13c"}' \
  go.getblock.io:443 protocol.Wallet/GetContract
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
channel = grpc.secure_channel('go.getblock.io:443', creds)
metadata = [('x-api-key', '<ACCESS-TOKEN>')]
stub = api_pb2_grpc.WalletStub(channel)

response = stub.GetContract(api_pb2.BytesMessage(value=bytes.fromhex('41a614f803b6fd780986a42c78ec9c7f77e6ded13c')), metadata=metadata)
print(response)
```
{% endcode %}
{% endtab %}

{% tab title="Go" %}
{% code title="example.go" %}
```go
conn, _ := grpc.Dial(
    "go.getblock.io:443",
    grpc.WithTransportCredentials(credentials.NewTLS(&tls.Config{})),
)
defer conn.Close()

client := api.NewWalletClient(conn)
ctx := metadata.AppendToOutgoingContext(
    context.Background(), "x-api-key", "<ACCESS-TOKEN>")

resp, _ := client.GetContract(ctx, &api.BytesMessage{Value: contractBytes})
fmt.Println(resp)
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Message

`SmartContract`

| Field             | Type   | Description                      |
| ----------------- | ------ | -------------------------------- |
| contract\_address | bytes  | The contract address             |
| origin\_address   | bytes  | The deployer address             |
| abi               | ABI    | The contract ABI, when available |
| name              | string | The contract name                |
| bytecode          | bytes  | The contract's deployed bytecode |

## Use Cases

* **ABI Retrieval**: Read a contract's ABI to encode calls
* **Contract Inspection**: Read a contract's name and deployer
* **Bytecode Reads**: Retrieve deployed bytecode
* **Verification**: Confirm a contract exists at an address

## Error Handling

| Code | Status            | Description                                                 |
| ---- | ----------------- | ----------------------------------------------------------- |
| 3    | INVALID\_ARGUMENT | A request field is missing or malformed                     |
| 5    | NOT\_FOUND        | The requested account, block, or transaction does not exist |
| 16   | UNAUTHENTICATED   | The access token is missing or invalid                      |
| 13   | INTERNAL          | The node failed to process the request                      |
