# invoke Method

Returns the result after calling a smart contract at scripthash with the given parameters.

> [!Note]
>
> - This method is to test your VM script as if they were ran on the blockchain at that point in time. This RPC call does not affect the blockchain in any way.
> - This method is provided by the plugin [RpcWallet](https://github.com/neo-project/neo-plugins/releases). You need to install the plugin before you can invoke the method.

## Parameter Description

scripthash: Smart contract scripthash

params: The parameters to be passed into the smart contract

## Example

Request body:

```json
{
  "jsonrpc": "2.0",
  "method": "invoke",
  "params": [
"dc675afc61a7c0f7b3d2682bf6e1d8ed865a0e5f",
[
  {
    "type": "String",
    "value": "name"
  },
  {
    "type":"Boolean",
    "value": false
  }
]
  ],
  "id": 1
}
```

Response body:

```json
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "script": "00046e616d65675f0e5a86edd8e1f62b68d2b3f7c0a761fc5a67dc",
        "state": "FAULT",
        "gas_consumed": "0.01",
        "stack": []
    }
}
```
