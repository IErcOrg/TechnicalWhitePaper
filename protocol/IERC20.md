## prefix
data:application/json,

## Simple example

### Deploy
data:application/json,{"p":"ierc-20","op":"deploy","tick":"ethi","max":"21000000","lim":"1000","wlim":"10000","dec":"8","nonce":"10"}

### Mint
data:application/json,{"p":"ierc-20","op":"mint","tick":"ethi","amt":"1000","nonce":"11"}

### Transfer
data:application/json,{"p": "ierc-20","op": "transfer","tick": "ethi","nonce": "45","to": [{"recv": "0x7BBAF8B409145Ea9454Af3D76c6912b9Fb99b2A9","amt": "10000"}]}

### Proxy transfer

data:application/json,{"p":"ierc-20","op":"proxy_transfer","proxy":[{"tick":"ethi","nonce":"20","from":"0x22222222222222222222222222222222222222222222","to":"0x22222222222222222222222222222222222222222222","amt":"333","value":"0.001","sign":"0x000"}]}

### Freeze sell

data:application/json,{"p":"ierc-20","op":"freeze_sell","freeze":[{"tick":"ethi","nonce":"1690469437811","platform":"0x33302dbff493ed81ba2e7e35e2e8e833db023333","seller":"0x22222222222222222222222222222222222222222222","amt":"333","value":"0.001","gasPrice":"33988168450","sign":"0x00"}]}

## Detailed examples

``` js

// data:application/json,
// +

// deploy; send 0eth from self to 0x0000000000000000000000000000000000000000;
{
    "p":"ierc-20", //protocol name: ierc20 | terc-20
    "op":"deploy", //operation: deploy/mint/transfer/freeze_sell/proxy_transfer
    "tick":"ethi", //token tick, can't be repeatable, case insensitive.
    "max":"21000000", //max supply
    "lim":"1000", //limit for each mint
    "wlim":"10000", //limit for each address can maximum mint, address balance < deploy.wlim (Before mint, please do not receive transfers from others, transfers are also counted as balance)
    "dec":"8", //decimal for minimum divie
    "nonce":"0", //increasing interger, suggest using address original nonce
}

// mint; send 0eth from self to 0x0000000000000000000000000000000000000000;
// The initiator of the transaction is the receiver
{
    "p":"ierc-20",
    "op":"mint", //mint operation
    "tick":"ethi",
    "amt":"1000", //mint amount
    "nonce":"1"
}

// transfer; send 0eth from self to 0x0000000000000000000000000000000000000000
{
  "p": "ierc-20",
  "op": "transfer", //transfer operation
  "tick": "ethi",
  "nonce": "2",
  "to": [ //batch transfer, the sum of amt must equal the previous amt param
    {
      "recv": "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48", //receiver address
      "amt": "50" //receiver amount
    },
    {
      "recv": "0x0000000000085d4780B73119b644AE5ecd22b376",
      "amt": "50"
    }
  ]
}
// proxy transfer; send 0eth from self to 0x0000000000000000000000000000000000000000 or 0x33302dbff493ed81ba2e7e35e2e8e833db023333 or platform address
{
  "p": "ierc-20",
  "op": "proxy_transfer", //transfer operation
  "proxy": [
    {
      "tick": "ethi", // tick
      "nonce": (+new Date()).toString(), // nonce
      "from": sellerAddress, // seller address, verify
      "to": "0x22222222222222222222222222222222222222222222", // buyer address (test)
      "amt": "333",
      "value": "0.001",
      "sign": sign // the agent must carry the signature to the chain, which can be confirmed
    }
  ]
}

// The buyer is buying and the seller's token is frozen;  send x eth from self to 0x0000000000000000000000000000000000000000 or 0x33302dbff493ed81ba2e7e35e2e8e833db023333 or platform address
{
  "p": "ierc-20",
  "op": "freeze_sell",
  "freeze": [
    {
      "tick": "ethi", // Seller Signature Information
      "nonce": (+new Date()).toString(), // Seller Signature Information
      "platform": "0x33302dbff493ed81ba2e7e35e2e8e833db023333", // Seller signature information: corresponding platform
      "seller": "0x22222222222222222222222222222222222222222222", // Freeze the corresponding seller
      "amt": "333", // Seller Signature Information
      "value": "0.001", // Seller Signature Information
      "gasPrice": "33988168450", // gasPrice
      "sign": sign // The seller authorizes the signature to verify the use
    }
  ]
}

// cancel approve; send 0eth from self to 0x0000000000000000000000000000000000000000
{
  "p": "ierc-20",
  "op": "cancel_approve",
  "sign": [
    {
        "message": "",
        "sign": "0xsign"
    }
  ]
}
```

## index

* Mint is invalid in the same block

### proxy_transfer

Signature Approval Example

``` ts
const message = JSON.stringify({
  title: 'ierc-20 one approve', // one approve
  to: '0x33302dbff493ed81ba2e7e35e2e8e833db023333', // platform address
  tick: 'ethi', // token
  amt: "333", // token amt
  value: "0.01", // eth value
  nonce: (+new Date()).toString(),
}, null, 4)
```