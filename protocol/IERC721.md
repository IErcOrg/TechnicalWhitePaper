# IERC-721



```js

// deploy; send 0eth from self to 0x0000000000000000000000000000000000000000;
{
    "p": "ierc-721",
    "op": "deploy",
    "max": "1000",
    "name": "collection name",
    "tick": "collection tick",
    "wlim":"1", 
    "blim":"30", // release mint amount
    "baseUrl": "http://aws.s3.com/{tokenid}.json"
}  


// mint; send 0eth from self to 0x0000000000000000000000000000000000000000;
{
    "p": "ierc-721",
    "op": "mint",
    "tokenId": "1",
    "tick": "",  // optional, which means uploading custom content
 	  "content-type": "text/xml", // optional, svg: "text/xml", img: "image/png;"
    "content": "<svg></svg>" // optional, data:image/jpg;base64,/9j/4QMZRXhpZgAASUkqAAgAAAAL....
}


// transfer; send 0eth from self to 0x0000000000000000000000000000000000000000;
{
    "p": "ierc-721",
    "op": "transfer",
  	"tick": "", // optional
    "to": [ //batch transfer
        {
          "recv": "", //receiver address
          "tokenId": "50",
        },
        {
          "recv": "", //receiver address
           "tokenId": "50",
        }
    ]
}


// proxy transfer; send 0eth from self to 0x0000000000000000000000000000000000000000 or seller address
{
  "p": "ierc-721",
  "op": "proxy_transfer", //transfer operation
  "proxy": [
    {
        "tick": "", // optional
        "tokenId": "1",
        "from": "0x1111", // seller address, verify
        "to": "0x22222222222222222222222222222222222222222222", // buyer address (test)
        "value": "0.001", // Ethereum price, float
        "nonce": "12222", // nonce
        "sign": "oxsign" // the agent must carry the signature to the chain, which can be confirmed,
    }
  ]
}


// The buyer is buying and the seller's token is frozen;  send x eth from self to 0x0000000000000000000000000000000000000000 or 0x33302dbff493ed81ba2e7e35e2e8e833db023333 or platform address
{
  "p": "ierc-721",
  "op": "freeze_sell",
  "freeze": [
    {
      "tick": "", // Seller Signature Information
      "tokenId": "tokenId", // Seller Signature Information
      "nonce": "", // Seller Signature Information
      "platform": "0x33302dbff493ed81ba2e7e35e2e8e833db023333", // Seller signature information: corresponding platform
      "seller": "0x22222222222222222222222222222222222222222222", // Freeze the corresponding seller
      "amt": "333", // Seller Signature Information
      "value": "0.001", // Seller Signature Information
      "gasPrice": "33988168450", // gasPrice
      "sign": "0xsign" // The seller authorizes the signature to verify the use
    }
  ]
}


// cancel approve; send 0eth from self to 0x0000000000000000000000000000000000000000
{
  "p": "ierc-721",
  "op": "cancel_approve",
  "sign": [
    {
        "message": "",
        "sign": "0xsign"
    }
  ]
}
```

## Sign

```js
// cancel 
const message = JSON.stringify({
    title: 'ierc-721 cancel approve', 
    to: DEX_ADDRESS, // platform address
    tick, // optional
    tokenId: "",
    nonce,
}, null, 4)

// approve
const message = JSON.stringify({
    title: 'ierc-721 one approve', // one approve
    to: DEX_ADDRESS, // platform address
    tick, // optional
    tokenId: "",
    amt: amt.toString(), // token amt
    value: value.toString(), // eth value
    nonce: (+new Date()).toString(),
}, null, 4)
```

## metadata
```js
{
    "title": "",
    "description": "",
    "image": "",
    "attributes": [ // Different project settings are different
        {
          "trait_type": "Ierc721", 
          "value": "true"
        }
    ]
}
```